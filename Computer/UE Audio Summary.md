# Unreal Engine Audio相关
### 播放按钮的点击音效
1.可以在按钮的Style蓝图中设置Clicked Sound和Hovered Sound。
2.在按钮的点击回调事件中，调用PlaySound2D()接口。
```c++
void UCustomClass::OnBtnClickedEvent()
{
    if (USoundWave* pSound = Cast<USoundWave>(GET_ASSET(TEXT("SoundWave'/Game/Blueprin)ts/BGM/UI_Click.UI_Click'"))))
    {
        pSound->bLooping = false;
        UGameplayStatics::PlaySound2D(GetWorld(), pSound, 1.f); 
    }
}
```