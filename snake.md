

### 初始化游戏



### 更新游戏状态

- 更新尾的位置
- 根据移动指令更新头的位置
- 判断位置是否合法
- 更新玩家得分和蛇的长度
- 更新水果位置

### 设置游戏难度（睡眠时间）

~~~cpp
// Function to set the game difficulty level 
int SetDifficulty() 
{ 
	int dfc, choice; 
	cout << "\nSET DIFFICULTY\n1: Easy\n2: Medium\n3: hard "
			"\nNOTE: if not chosen or pressed any other "
			"key, the difficulty will be automatically set "
			"to medium\nChoose difficulty level: "; 
	cin >> choice; 
	switch (choice) { 
	case '1': 
		dfc = 50; 
		break; 
	case '2': 
		dfc = 100; 
		break; 
	case '3': 
		dfc = 150; 
		break; 
	default: 
		dfc = 100; 
	} 
	return dfc; 
}

~~~





### 处理用户输入

```cpp
// Function to handle user UserInput 
void UserInput() 
{ 
	// Checks if a key is pressed or not 
	if (_kbhit()) { 
		// Getting the pressed key 
		switch (_getch()) { 
		case 'a': 
			sDir = LEFT; 
			break; 
		case 'd': 
			sDir = RIGHT; 
			break; 
		case 'w': 
			sDir = UP; 
			break; 
		case 's': 
			sDir = DOWN; 
			break; 
		case 'x': 
			isGameOver = true; 
			break; 
		} 
	} 
}

```

1. `_kbhit()` 函数检查控制台是否有最近的击键。 如果函数返回非零值，则表明缓冲区中正在等待按键操作。 然后程序可以调用 `_getch()` 或 `_getche()` 来获取击键。

2. `_getch()`是一个非标准函数，存在于最常使用的文件中。 它不是 C 标准库或 ISO C 的一部分，也不是 POSIX 定义的



