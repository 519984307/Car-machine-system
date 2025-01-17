# project framework

## 项目框架

### 1. 项目概述

- **项目名称**: 新能源车智慧车机系统
- **项目目标**: 开发一个集成多种功能的智慧车机系统，包括导航、娱乐、驾驶辅助、安全监控等，以提升驾驶体验，提高车辆安全性，实现智能管理。

### 2. 项目功能模块

1. **开机动画和界面设计** 
   - 使用QT的UI界面实现所有GUI设计。
   - 启动时显示项目主题的开机动画。
   - 界面布局合理、美观，界面切换流畅。
2. **系统联网功能**
   - 车机系统启动后自动联网获取北京时间和本地天气信息。
   - 天气信息和系统时间显示在主界面上，根据实时天气信息更新天气图标。
3. **语音控制**
   - 点击语音控制APP或使用原车语音按键唤醒声控。
   - 支持“你好小X”语音唤醒，内置常用语音指令，语音识别并显示在语音界面上。
4. **音频播放器**
   - 实现音乐播放、暂停、上/下一首、进度显示、音量控制等功能。
   - 读取指定路径下的音频文件，播放时动态显示背景。
5. **流量充值服务**
   - 查看当前车机系统剩余流量，用户可以扫码登录并充值流量。
   - 流量充值通过扫码支付，数据通过云服务器实现。
6. **道路救援服务**
   - 实现一键道路救援功能，发送短信通知指定用户。
   - 利用API接口获取位置并发送。
7. **倒车影像**
   - 支持倒车影像功能，实时显示摄像头图像。
8. **座椅调节**
   - 实现座椅通风/加热功能界面，可调节风量等。
9. **附加功能**
   - 用户自行拓展其他实用性功能，确保项目运行稳定无BUG。
   - 编写项目测试文档、项目功能文档、README及拍摄项目演示视频，提交项目源码。

### 3. 服务器功能模块

1. **用户信息管理**
   - 用户扫码登录，云服务器识别并存储用户信息。

2. **支付信息管理**
   - 扫码支付流量充值，云服务器确认金额并存储支付信息。

3. **GUI设计**
   - 使用QT进行服务器端GUI设计。
   - 管理员账号登录查看后台数据。

4. **数据传输**
   - 客户端发送和接收数据均使用JSON格式，确保数据传输的稳定性和可靠性。

### 4. 实现思路

#### 1. 项目初始化

- 创建QT项目，设置项目的基本配置，包括窗口大小、标题等。
- 设计模式：状态模式（开始start，运行run，待机standby）

#### 2. 开机动画

- 在QT项目中使用FFmpeg来实现开机动画的展示，可以通过定时器和帧动画实现。
- 确保动画展示符合项目主题。

#### 3. 界面设计

- 使用QT Widgets设计主界面和各功能模块界面。
- 确保界面布局合理，切换流畅。

#### 4. 天气显示

- 使用HTTP连接心知天气API实现获取天气数据功能。
- 获取广州时间和本地天气信息，并更新到主界面上。

#### 5. 语音控制

- 集成语音识别库，如讯飞语音识别API。
- 实现语音唤醒、识别和指令执行功能。

#### 6. 音频播放器

- 集成FFmpeg音频解码库，实现音频播放功能。
- 使用QT Widgets设计音频播放器界面。

#### 7. 流量充值

- 实现用户扫码登录和充值功能。
- 使用QT网络模块与云服务器通信，实现充值和支付功能。

#### 8. 道路救援

- 实现一键道路救援功能，发送短信通知。
- 使用API接口获取当前位置。

#### 9. 倒车影像

- 集成摄像头模块，实时获取图像并显示在车机界面上。

#### 10. 座椅调节

- 设计座椅调节界面，实现通风/加热等功能。

#### 11. 数据库管理

- 使用Qt自带的SQL模块(QSqlDatabase, QSqlQuery)管理用户信息和支付信息。
- 实现本地数据的存储和读取。

### 5. 编译与测试

- 编写CMakeLists.txt实现自动化编译。
- 完成项目后编写测试文档和功能文档。

通过以上的项目框架和实现思路，可以构建一个完整的基于物联网技术的新能源车智慧车机系统。





<<<<<<< Updated upstream
=======
### 设计模式

>>>>>>> Stashed changes
#### 1. 状态模式（系统启动与开机动画）

```cpp
// State.h
class Context;
//状态类
class State {
public:
    virtual ~State() {}
    virtual void handle(Context* context) = 0;
};
//环境类（上下文）
class Context {
private:
    State* state;
public:
    Context(State* state) : state(state) {}
    ~Context() { delete state; }
    void setState(State* state) {
        delete this->state;
        this->state = state;
    }
    void request() {
        state->handle(this);
    }
};
//具体状态类
// ConcreteStates.h
class StartState : public State {
public:
    void handle(Context* context) override;
};

class RunningState : public State {
public:
    void handle(Context* context) override;
};

class StopState : public State {
public:
    void handle(Context* context) override;
};

// ConcreteStates.cpp
#include "State.h"
#include "ConcreteStates.h"
#include <iostream>

void StartState::handle(Context* context) {
    std::cout << "System is starting...\n";
    context->setState(new RunningState());
}

void RunningState::handle(Context* context) {
    std::cout << "System is running...\n";
    context->setState(new StopState());
}

void StopState::handle(Context* context) {
    std::cout << "System is stopping...\n";
    context->setState(new StartState());
}

// Main.cpp
#include "State.h"
#include "ConcreteStates.h"

int main() {
    Context* context = new Context(new StartState());
    context->request();
    context->request();
    context->request();
    delete context;
    return 0;
}
```