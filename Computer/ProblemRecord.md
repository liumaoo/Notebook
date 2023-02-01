## 开发问题记录（待解决）

#### UE自动检测脚本问题

UE中编辑器加载资源接口：

```c++
/**
  * Load an asset from the Content Browser. It will verify if the object is already loaded and only load it if it's necessary.
  * @param	AssetPath		Asset Path of the asset (that is not a level).
  * @return	Found or loaded asset.
  */
UFUNCTION(BlueprintCallable, Category = "Editor Scripting | Asset")
static UObject* LoadAsset(const FString& AssetPath);
```

遍历文件夹中的特定文件：

```c++
FString BasePath = FPaths::Combine(*FPaths::ProjectDir(), TEXT("Content/")); 
FString FilePath = FPaths::Combine(*strBasePath, TEXT("UI/")); 
TArray<FString> FilePathArray;
IFileManager::Get().FindFilesRecursive(FilePathArray, *FilePath, TEXT("*.uasset"), true, false);
//递归找到文件夹中所有相匹配的文件
for (const auto& ItsPath : FilePathArray)
{
    //例：FilePath = “../../../../ProjectDir/Content/UI/TestUI.uasset”
    ItsPath.FindLastChar('/', StartIndex);
    ItsPath.FindLastChar('.', EndIndex);
    FString FileName = ItsPath.Mid(StartIndex + 1, EndIndex - StartIndex - 1); //取文件名
 	FString LoadObjectPath = ItsPath.Replace(*BasePath, TEXT("/Game/")).Replace(TEXT(".uasset"), *FString::Printf(TEXT(".%s"), *FileName))
    //用于加载UObject的路径，例：/Game/UI/TestUI.TestUI
    FString LoadObjectPath = ItsPath.Replace(*BasePath, TEXT("/Game/")).Replace(TEXT(".uasset"), *FString::Printf(TEXT(".%s"), *FileName)) + "_C";
    //用于加载UClass的路径，例：/Game/UI/TestUI.TestUI_C
}
```

游戏引擎架构中，编辑器的设计和接口调用，如何使用外部程序调用引擎中的接口进行修改？