# Game Programming Pattern

##  设计模式

### 1. 命令模式

> 定于：*将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化； 对请求排队或记录请求日志，以及支持可撤销的操作。*
>
> * **命令是具现化的方法调用。**
> * **命令模式是一种回调的面向对象实现。**

* 当你有接口只包含一个没有返回值的方法时，很可能你可以使用命令模式。

```cpp
// 基类
class Command
{
public:
  virtual ~Command() {}
  virtual void execute() = 0;
};

// 子类
class JumpCommand : public Command
{
public:
  virtual void execute() { jump(); }
};

class FireCommand : public Command
{
public:
  virtual void execute() { fireGun(); }
};

class InputHandler
{
public:
  void handleInput();

  // 绑定命令的方法……

private:
  Command* buttonX_;
  Command* buttonY_;
  Command* buttonA_;
  Command* buttonB_;
};
void InputHandler::handleInput()
{
  if (isPressed(BUTTON_X)) buttonX_->execute();
  else if (isPressed(BUTTON_Y)) buttonY_->execute();
  else if (isPressed(BUTTON_A)) buttonA_->execute();
  else if (isPressed(BUTTON_B)) buttonB_->execute();
}
```

**使用场景：**

1. 配置输入：按键、鼠标、按钮的行为
2. 角色行为
3. 撤销和重做
