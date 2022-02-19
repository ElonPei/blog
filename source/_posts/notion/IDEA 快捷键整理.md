---
categories:
  - 开发工具
created_time: February 19, 2022 9:30 AM
date: 2017/11/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: IDEA 快捷键整理
---


### Analyze(代码分析)

- RunInspection command `shift alt I` Analyze->Run Inspection By Name 根据代码检查的例子，运行代码检查

### Code菜单

- AutoIndentLines `control alt I` Code->Auto-Indent Lines 自动缩进
- CommentByBlockComment `command alt /` Code->Comment With Block Comment 整段用/* */注释
- CommentByLineComment `command /` Code->Comment with Line Comment 注释
- CodeCompletion `control SPACE` > `command SPACE` Code->Completion 代码自动完成
- SmartTypeCompletion `control shift SPACE` > `command shift SPACE`Code->Completion->SmartType 智能类型完成
- CollapseBlock `command shift .` Code->Folding 折叠区块代码
- CollapseSelection `command .` Code->Folding 选中某一段代码后，用这个快捷键来折叠
- CollapseRegion `command -` Code->Folding->Collapse 选中后，折叠某个方法或类
- CollapseAll `command -` Code->Folding->Collapse 选中后，折叠某个方法或类
- CollapseAllRegions `command shift -` Code->Folding->Collapse All 折叠所有 比如折叠某个类的某个方法
- CollapseRegionRecursively `command alt -` Code->Folding->Collapse Recursively 循环折叠
- ExpandAll `command +|control =` Code->Folding->Expand 展开当前方法
- ExpandAllRegions `command shift +` Code->Folding->Expand All 展开所有被折叠的地方
- ExpandRegionRecursively `command alt +` Code->Folding->Expand Recursively 迭代展开方法
- ImplementMethods `control I` Code->Implement Method… 实现某个方法
- InsertLiveTemplate `command J` Code->Insert Live Template 插入代码模板，比如sout等
- MoveLineDown `alt shift DOWN` Code->Move Line Down 移动当前行到上一行
- MoveLineUp `alt shift UP` Code->Move Line Up 移动当前行到下一行
- MoveStatementDown `command shift DOWN` Code->Move Statement Down 把当前代码块，比如一个方法往下移动
- MoveStatementUp `command shift UP` Code->Move Statement Up 把当前代码块，比如一个方法往上移动
- OptimizeImports `control alt O` Code->Optimize Imports import优化
- OverrideMethods `control O` Code->Override Method 重载当前类的方法
- ReformatCode `command alt L` Code->Reformat Code 格式化代码
- SurroundWith `command alt T` Code->Surround With 选中某一段，然后可以用if之类的包含这段
- Unwrap `command shift DELETE` Code->Unwrap 删除，但是没有尝试出来
- Generate `command N` Code->generate 代码生成，包括构造函数，getter setter
- ContextHelp `F1` 显示方法的帮助信息，比如javadoc
- SurroundWithLiveTemplate `command alt J` 选中一行或一端，然后用模版来包裹

### Edit菜单

- CopyPaths `command shift C` Edit->Copy Path 拷贝当前文件的路径
- CopyReference `command alt shift C` Edit->Copy Reference 拷贝引用
- $Cut `command X` Edit->Cut 剪切
- EditorDuplicate `command D` Edit->Duplicate Line 复制当前行到下一行
- FindNext `F3|command G` Edit->Find->Find Next 找到下一个
- SelectNextOccurrence `control G` Edit->Find->Find Next 选定某些个字符后，找到下一个出现的地方
- FindPrevious `command shift G` Edit->Find->Find Previous 查找某个变量后，找到上一个出现的位置
- ShowSettingsAndFindUsages `command shift alt F7` Edit->Find->Find Usage Settings 打开find usages的设置 需要选中某个类，方法，变量才能执行
- FindUsages `alt F7` Edit->Find->Find Usages 找到类，方法或变量被使用的地方，在hierarchy中显示出来
- FindUsagesInFile `command F7` Edit->Find->Find Usages in File 给定一个方法或变量，在当前文件中找到用的地方
- FindInPath `command shift F` Edit->Find->Find in Path 在文件中查找
- HighlightUsagesInFile `command shift F7` Edit->Find->Highlight Usages in File 选中某个变量，然后进行查找
- Replace `command R` Edit->Find->Replace 替换
- ReplaceInPath `command shift R` Edit->Find->Replace in Path 根据文件的路径，替换文件中的内容，可以做到全局替换
- SelectAllOccurrences `command control G` Edit->Find->Select All Occurrences 选定某些个字符后，在当前文件中，找到所有该字符 并高亮显示
- ShowUsages `command alt F7` Edit->Find->Show Usages 在当前editor，显示类，方法或变量被使用的地方
- UnselectPreviousOccurrence `control shift G` Edit->Find->Unselect Occurrence 通过Select Next Occurrence找到一些内容后，不选定这些内容
- EditorIndentSelection `TAB` Edit->Indent Selection 把选中的内容进行缩进

### Editor Actions

- EditorCutLineEnd `control K` Editor Actions->Cut up to Line End 剪切光标之后的到行尾的内容
- EditorCodeBlockEnd `command alt }` Editor Actions->Move Caret to Code Block End 移动光标到代码块的结尾，比如移动到方法的最后一个}处
- EditorLineEnd `command RIGHT` Editor Actions->Move Caret to Line End 光标移动到行尾
- EditorLineEndWithSelection `shift END` Editor Actions->Move Caret to Line End With Selection 光标移动到行尾，并全选
- EditorLineStart `command LEFT` Editor Actions->Move Caret to Line Start 移动光标到行开头，很有用
- EditorLineStartWithSelection `command shift LEFT` Editor Actions->Move Caret to Line Start with Selection 光标向左移动到行开始处，并全选
- EditorPreviousWord `alt LEFT` Editor Actions->Move Caret to Previous Word 移动光标到上一个单词
- EditorTextEnd `command END` Editor Actions->Move Caret to Text End 移动光标到文件的最后面
- EditorTextEndWithSelection `command shift END` Editor Actions->Move Caret to Text End with Selection 选中从光标开始之后的内容
- EditorTextStartWithSelection `command shift HOME` Editor Actions->Move Caret to Text Start with Selection 选中从光标之前的所有内容
- EditorMoveToPageBottom `command PAGE_DOWN` Editor Actions->Move caret to Page Bottom 移动光标到文件底部
- EditorMoveToPageTop `command PAGE_UP` Editor Actions->Move caret to Page Top 移动光标到文件顶部
- NextTemplateVariable `TAB|ENTER` Editor Actions->Next Template Variable 下一个模板
- EditorUnSelectWord `alt DOWN` Editor Actions->Shrink Selection 缩小已经选中的
- EditorSplitLine `command ENTER` Editor Actions->Split Line 光标停留在回车处
- EditorStartNewLine `shift ENTER` Editor Actions->Start New Line 在本行之后开始一个新行，很有用的键
- EditorStartNewLineBefore `command alt ENTER` Editor Actions->Start New Line Before Current 在本行之前开始一个新行
- EditorToggleCase `command shift U` Editor Actions->Toggle Case 转换为大写
- EditorUnindentSelection `shift TAB` Editor Actions->Unindent Line or Selection 缩进回退
- EditorJoinLines `control shift J` Editor Join Lines 下一行与本行放在同一行中
- EditorTextStart `command HOME` 回到文件的第一行，非常有用

### File菜单

- ShowSettings `command ,` Intellij IDEA->preferences 打开idea的设置
- JumpToLastChange `command shift BACK_SPACE`
- EditorNextWord `alt RIGHT` Move Caret to Next Word 移动光标到下一个单词
- Synchronize `command alt Y` File->Synchronize 同步文件

### Navigate菜单

- Back `command OPEN_BRACKET` Navigate->Back 回退
- NextOccurence `command alt DOWN` Navigate->Bookmarks->Next Occurence 下一个书签
- ShowBookmarks `command F3` Navigate->Bookmarks->Show Bookmarks 显示书签
- ToggleBookmark `F3` Navigate->Bookmarks->Toggle Bookmark 加书签
- ToggleBookmarkWithMnemonic `alt F3` Navigate->Bookmarks->Toggle Bookmark With Mnemoic 打有标示的标签
- CallHierarchy `control alt H` Navigate->Call Hierarchy 选中某个方法后，打开该方法的调用链
- GotoClass `command O` Navigate->Class 打开某个类
- GotoDeclaration `command B` Navigate->Declaration 调到变量或方法声明处
- GotoFile `command shift O` Navigate->File 打开一个文件
- ShowFilePath `command alt F12` Navigate->File Path 显示文件路径
- FileStructurePopup `command F12` Navigate->File Structure 打开文件的结构 如果是class文件，显示变量，方法；也可以xml文件的结构
- GotoImplementation `command alt B` Navigate->Implementations(s) 调到该方法的实现处
- GotoLine `command L` Navigate->Line 跳到某一行
- MethodHierarchy `command shift H` Navigate->Method Hierarchy 不知道 方法结构，貌似不起作用
- GotoNextError `F2` Navigate->Next Highlighted Error 光标移到下一个错误
- GotoPreviousError `shift F2` Navigate->Previous Highlighted Error 跳到上一个错误处
- PreviousOccurence `command alt UP` Navigate->Previous Occurence 调用关系跳转
- SelectIn `alt F1` Navigate->Select In 可以进入Project View File Structure
- GotoSuperMethod `control U` Navigate->Super Method 跳到父类方法
- GotoSymbol `command alt O` Navigate->Symbol 根据符号名打开文件
- GotoTest `command shift T` Navigate->Test 跳到[测试](http://lib.csdn.net/base/softwaretest)方法，或创建一个测试方法
- GotoTypeDeclaration `command shift B|control shift B` Navigate->Type Declaration 跳到变量的声明处
- TypeHierarchy `control H` Navigate->Type Hierarchy 打开类型结构图
- NewElementSamePlace `control alt N` 新建各种文件

### 其他

- AddToFavoritesPopup `alt shift F` Other->Add to Favorites
- ProjectViewChangeView `alt F1` Other->Change View 切换view
- ClassNameCompletion `control alt SPACE` Other->Class Name Completion 类名自动完成
- FileChooser.GotoDesktop `command D` Other->Desktop Directory
- ExportToTextFile `control O` Other->Export to Text File 导出到文本文件中
- EditBreakpoint `command shift F8` Other->Edit 光标在断点所在行以后，可以编辑断点，比如加入条件
- CodeInspection.OnEditor `alt shift I` Other->Inspect Code With Editor Settings
- EditorLookupDown `control DOWN` Other->Lookup down
- EditorLookupUp `control UP` Other->Lookup up
- MaintenanceAction `command alt shift SLASH` Other->Maintenance
- Console.Open `command shift F10` Other->Open Console 打开console
- EditSourceInNewWindow `shift F4` Other->Open Source in New Window 用新窗口打开文件
- PreviousEditorTab `control shift LEFT` Other->Previous Editor Tab
- Rerun `command R` Other->Rerun 再次运行
- RerunTests `control command R` 再次运行测试
- ShowIntentionActions `alt ENTER` Other->Show Intention Actions 自动完成功能，常用功能
- ShowReformatFileDialog `command shift alt L` Other->Show Reformat File Dialog 显示格式化代码的对话框
- Switcher `ctrl TAB|ctrl shift TAB` Other->Switcher 弹出列表，可以在打开的文件列表中切换
- ActivateTerminalToolWindow `alt F12` Other->Terminal 打开终端
- Console.History.Browse `command alt E` 展示console中的历史

### Refactor(重构)

- ChangeTypeSignature `command shift F6` 类型迁移
- ChangeSignature `command F6` Refactor->Change Signature 修改方法
- CopyElement `F5` Refactor->Copy 拷贝，比如拷贝一个类
- IntroduceConstant `command alt C` Refactor->Extract->Constant 重构，抽出一个constant
- IntroduceField `command alt F` Refactor->Extract->Field 把方法中的一个变量抽成一个类变量
- ExtractMethod `command alt M` Refactor->Extract->Method 重构，代码抽成方法
- IntroduceParameter `command alt P` Refactor->Extract->Parameter 重构，把某个值重构成方法的参数
- IntroduceVariable `command alt V` Refactor->Extract->Variable 自动引入变量
- Inline `command alt N` Refactor->Inline 内联，把方法的内容放入到调用者中 也就是减少一个方法
- Move `F6` Refactor->Move 移动方法或者类
- RenameElement `shift F6` Refactor->Rename 重构，命名元素(变量名，方法名)
- SafeDelete `command DELETE` Refactor->Safe Delete 重构，安全删除元素(变量，方法等)
- Refactorings.QuickListPopupAction `control T` 重构提示

### Build菜单

- CompileDirty `command F9` Build->Make Project 编译工程
- Compile `command shift F9` Build->Compile 编译当前文件

### Run菜单

- Debug `control D` Run->Debug 调试上一次调试过的程序
- DebugClass `control shift F9|control shift D` 调试类
- ChooseDebugConfiguration `control alt D` Run->Debug… 选择哪一个程序或方法来调试
- EvaluateExpression `alt F8` Run->Evaluate Expression 调试，表达式估值
- ForceRunToCursor `command alt F9` Run->Force Run to Curosr 在调试中，强制让程序运行到光标所在处
- ForceStepInto `alt shift F7` Run->Force Step Into debug时，强制进入
- ForceStepOver `alt shift F8` Run->Force Step Over debug时，强制Step Over
- QuickEvaluateExpression `command alt F8` Run->Quick Evaluate Expression 调试，快速表达式估值
- Resume `command alt R` Run->Resume 调试，继续，放过本断点直到下一个断点
- Run `control R` Run->Run 运行上一次运行的程序
- RunToCursor `alt F9` Run->Run to Cursor 在调试中，让程序运行到光标所在处
- ChooseRunConfiguration `control alt R` Run->Run… 选择哪一个程序或方法来运行
- RunClass `control shift F10|control shift R` Run->Run… 运行某个方法
- ShowExecutionPoint `alt F10` Run->Show Execution Point 回到断点处
- SmartStepInto `shift F7` Run->Smart Step Into debug时，可以选择哪个方法step into
- StepInto `F7` Run->Step Into 调试 进入方法
- StepOut `shift F8` Run->Step Out 跳过本方法
- StepOver `F8` Run->Step Over 调试时，跳过某个方法
- Stop `command F2` Run->Stop 停止程序运行
- ToggleLineBreakpoint `command F8` Run->Toggle Line Breakpoint 打断点
- ViewBreakpoints `command shift F8` Run->View Breakpoints 列出所有断点/

### Version Control System

- Vcs.ShowMessageHistory `command E` Version Control System->Commit Message History
- GotoChangedFile `command O` Version Control System->Diff->[Go](http://lib.csdn.net/base/go) To Changed File
- NextDiff `F7` Version Control System->Diff->Next Difference 比较两个文件时，下一个不同的地方
- Diff.ShowDiff `command D` Version Control System->Diff->Show Diff
- Diff.ShowSettingsPopup `command shift D` Version Control System->Diff->Show Setting Popup
- VcsShowNextChangeMarker `shift control alt DOWN` Version Control System->Next Change
- VcsShowPrevChangeMarker `shift control alt UP` Version Control System->Previous Change
- Vcs.QuickListPopupAction `ctrl V` Version Control System->Vcs Operations Popup 打开vcs的菜单
- PreviousDiff `shift F7` Version Control Systems->Diff->Previous Difference 上一个不同的地方
- ShelveChanges.UnshelveWithDialog `command shift U` Version Control Systems->Shelve->Unshelve Changes
- ToggleTemporaryLineBreakpoint `command shift alt F8` 加一个临时的行断点

### View菜单

- CompareTwoFiles `command D` View->Compare With 选择一个文件与当前文件进行比较
- EditorContextInfo `control shift Q` View->Context Info 显示当前编辑文件的信息 也就是类信息
- ShowErrorDescription `command F1` View->Error Description 显示错误信息
- ExternalJavaDoc `shift F1` View->External Documentation 不知道
- ParameterInfo `command P` View->Parameter Info 显示某个方法的参数信息
- QuickJavaDoc `F1` View->Quick Documentation 选中某个方法或变量后，打开文档
- RecentChangedFiles `command shift E` View->Recent Changed Files 最近修改过的文件列表
- RecentFiles `command E` View->Recent Files 最近打开的文件列表
- EditSource `F4` View->Show source 进入源码,进入方法的定义
- QuickImplementations `command Y` 查看方法的实现

### Window菜单

- CloseActiveTab `control shift F4` Window->Active Tool Window->Close Active Tab 关闭活跃的Tab
- HideActiveWindow `shift ESC` Window->Active Tool Window->Hide Active Tool Window 隐藏打开的工具栏 也比较有用
- HideAllWindows `command shift F12` Window->Active Tool Window->Hide All Tool Windows 隐藏所有工具栏
- JumpToLastWindow `F12` Window->Active Tool Window->Jump to Last Tool Window 回到上一个工具窗
- MaximizeToolWindow `command shift '` Window->Active Tool Window->Maximize Tool Window 最大化工具栏，在看调试信息的时候，还是比较有用
- ResizeToolWindowDown `command shift DOWN` Window->Active Tool Window->Resize->Stretch to Down 工具栏下边框往下移动
- ResizeToolWindowLeft `command shift LEFT` Window->Active Tool Window->Resize->Stretch to Left 工具栏左边框往左移动
- ResizeToolWindowRight `command shift RIGHT` Window->Active Tool Window->Resize->Stretch to Right 工具栏右边框往右移动
- ResizeToolWindowUp `command shift UP` Window->Active Tool Window->Resize->Stretch to Up 工具栏上边框往上移动
- PreviousTab `command shift [` Window->Editor Tab->Select Previous Tab editor 的前一个tab tab切换
- NextProjectWindow `command BACK_QUOTE` Window->Next Project Window 切换到上一个工程
- PreviousProjectWindow `command shift BACK_QUOTE` Window->Previous Project Window 切换到下一个工程
- RestoreDefaultLayout `shift F12` Window->Restore Default Layout 恢复默认布局，这个还是比较常用