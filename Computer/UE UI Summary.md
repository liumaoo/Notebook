## Slate源码分析

先在游戏线程（主线程）中执行OnPaint，获取LayerID和控件的信息，得到一个FSlateDrawElement，之后在渲染线程中根据LayerID合批渲染。

#### 结论：

1. 如果想新建一个自己的UI类型，直接派生一个SWidget子类然后重载OnPaint，然后再在UMG层建一个与之对应的简单封装即可。

2. 如果想要在UI上做一些花样，比如动态UI，3D UI可以在渲染层的vertexshader或者pixleshader上做文章，可以自己给UI写一些特殊shader。

3. 想要优化UI，也是在渲染层做优化，不过unreal的UI已经自带batch，剔除等优化操作了。

#### UI控件的两套位置变换

当控件作为CanvasPanelSlot时，遵循Slot的位置大小和锚点。

但每个控件本身都有一个RenderTransform，可以控制控件本身的平移，缩放，倾斜和旋转。

#### 鼠标输入事件





```c++
//Controller中也有鼠标点击事件回调（点击3D世界）
void APlayerController::MouseClickEvent(const FHitResult& InHitResult, const FKey& InKey)
{
    //InHitResult可以获取点击位置，InKey可以获取键盘值
}
```



#### NativePaint

画线和画虚线

```c++
//重写NativePaint，与Tick()类似，每帧调用用于渲染
int32 NativePaint(const FPaintArgs& Args, const FGeometry& AllottedGeometry, const FSlateRect& MyCullingRect, FSlateWindowElementList& OutDrawElements, int32 LayerId, const FWidgetStyle& InWidgetStyle, bool bParentEnabled) const
{
    //球的轨迹线
	if (TrackLinePoints.Num() > 0)
	{
        //FSlateDrawElement::MakeLines()画出TrackLinePoints中的所有点
		FSlateDrawElement::MakeLines(
			OutDrawElements,
			++LayerId,
			AllottedGeometry.ToPaintGeometry(),
			TrackLinePoints,
			ESlateDrawEffect::None,
			FLinearColor(0.9f, 0.1f, 0.1f, 1.f),
			true,
			4.f);
	}
    
    //球到目标点的虚线
	if (TargetToBallBrokenLine.Num() > 0 && TargetToBallBrokenLine.Num() % 2 == 0)
	{
		FPaintContext Context(AllottedGeometry, MyCullingRect, OutDrawElements, LayerId, InWidgetStyle, bParentEnabled);
        //UWidgetBlueprintLibrary::DrawLine()，画出两点之间的线段
		for (int32 i = 0; i < TargetToBallBrokenLine.Num(); i += 2)
		{
			UWidgetBlueprintLibrary::DrawLine(
				Context,
				TargetToBallBrokenLine[i],
				TargetToBallBrokenLine[i + 1],
				FLinearColor::White,
				true,
				1.f);
		}
	}
}

//计算两个点之间的所有虚线段
void ComputeBrokenLine(TArray<FVector2D>& OutLine, const FVector2D& InStartPos, const FVector2D& InEndPos)
{
	constexpr float DeltaLen = 4.f; //设置虚线间的间隔
	FVector2D DirectionVector = InEndPos - InStartPos;
	const int32 Count = DirectionVector.Size() / DeltaLen;
	DirectionVector.Normalize();
	const FVector2D DeltaLenVector = DeltaLen * DirectionVector;
	for (int32 i = 0; i < Count; i += 2)
	{
		FVector2D StartPoint = InStartPos + i * DeltaLenVector;
		FVector2D EndPoint = InStartPos + (i + 1) * DeltaLenVector;
		OutLine.Add(StartPoint);
		OutLine.Add(EndPoint);
	}
}
```



FVector2D重载了 | 为点积（dot product），重载了 ^ 为叉积（cross product）。

Pitch 俯仰角，Yaw 偏航角， Roll 翻转角。

ortho 正交
