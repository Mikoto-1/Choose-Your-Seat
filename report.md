# 电影院选座系统 - 项目开发报告

## 📋 项目基本信息

**项目名称**: 电影院选座系统 (Choose-Your-Seat)  
**开发者**: 清华大学Web前端技术实训  
**开发时间**: 2025年6月-7月  
**当前版本**: v1.3-stable  
**项目类型**: 前端Web应用  
**技术栈**: HTML5, CSS3, JavaScript (ES6+), Canvas 2D API  

## 🎯 项目概述

本项目是一个基于HTML5 Canvas技术开发的现代化电影院选座系统，提供了完整的在线选座、购票、用户管理等功能。系统采用弧形座位布局设计，模拟真实影院环境，支持个人票和团体票两种购票模式，并实现了严格的年龄限制规则和智能选座算法。

### 主要特性

- **🎨 可视化选座**: 基于Canvas的弧形座位布局，8种座位状态实时显示
- **👤 用户管理**: 完整的注册/登录系统，数据持久化存储
- **🎫 智能购票**: 支持个人票和团体票，自动选座算法
- **⚡ 实时反馈**: 年龄限制实时UI反馈，操作状态即时更新
- **📱 响应式设计**: 适配桌面端和移动端，流畅的用户体验

## 🛠️ 实现思路

### 1. 系统架构设计

#### 1.1 技术选型

- **前端框架**: 原生JavaScript (Vanilla JS)
- **绘图引擎**: HTML5 Canvas 2D API
- **数据存储**: localStorage (浏览器本地存储)
- **样式设计**: CSS3 + Flexbox + Grid
- **架构模式**: 单页应用 (SPA)

#### 1.2 核心架构

```txt
电影院选座系统
├── 用户管理模块
│   ├── 注册/登录功能
│   ├── 用户状态管理
│   └── 数据持久化
├── 座位管理模块
│   ├── Canvas绘制引擎
│   ├── 座位状态管理
│   └── 交互事件处理
├── 票务操作模块
│   ├── 选座逻辑
│   ├── 购票流程
│   └── 价格计算
└── UI/UX模块
    ├── 响应式布局
    ├── 视觉反馈
    └── 用户体验优化
```

### 2. 核心功能实现

#### 2.1 Canvas座位绘制系统

**弧形布局算法**:

```javascript
// 极坐标转换算法
const currentAngle = startAngle + col * angleStep;
const x = arcCenter.x + Math.cos(currentAngle) * currentRadius;
const y = arcCenter.y + Math.sin(currentAngle) * currentRadius;
```

**关键实现**:

- 使用极坐标系统实现弧形座位布局
- Canvas坐标变换实现座位旋转对银幕
- 多层绘制：座位背景 → 边框 → 细节 → 标记
- 8种座位状态的颜色编码系统

#### 2.2 用户管理系统

**数据结构设计**:

```javascript
// 用户数据结构
const userStructure = {
    username: "用户名",
    password: "密码",
    email: "邮箱",
    tickets: [], // 票据记录
    registeredAt: "注册时间"
};
```

**核心功能**:

- 表单验证：用户名格式、密码强度、邮箱验证
- 安全存储：localStorage数据持久化
- 会话管理：自动登录状态恢复
- 用户隔离：票据数据按用户ID分离

#### 2.3 智能选座算法

**个人票选座算法**:

```javascript
// 距离优化算法
const distance = Math.abs(seat.col - centerCol) + Math.abs(seat.row - centerRow);
preferredSeats.sort((a, b) => a.distance - b.distance);
```

**团体票连座算法**:

```javascript
// 连续座位扫描算法
for (let startCol = 1; startCol <= maxCols - groupSize + 1; startCol++) {
    // 检查连续可用性
    let consecutiveSeats = [];
    for (let i = 0; i < groupSize; i++) {
        // 验证座位可用性
    }
}
```

#### 2.4 年龄限制系统

**实时限制检查**:

- 少年(15岁以下)：禁止前3排
- 老年人(60岁以上)：禁止后3排
- 团体票：综合所有成员的年龄限制
- UI实时反馈：灰色+🚫标记显示受限座位

### 3. 数据管理策略

#### 3.1 状态管理

```javascript
// 全局状态管理
let globalState = {
    seats: [],              // 座位数据
    selectedSeats: [],      // 选中座位
    reservedSeats: new Set(), // 预订座位
    soldSeats: new Set(),   // 已售座位
    currentUser: null,      // 当前用户
    userTickets: new Map()  // 用户票据
};
```

#### 3.2 数据持久化

- **用户数据**: localStorage.cinemaUsers
- **登录状态**: localStorage.currentUser
- **票据数据**: 集成在用户数据中
- **会话恢复**: 页面刷新后自动恢复状态

## 📖 使用说明

### 1. 系统启动

1. 直接在浏览器中打开 `index.html` 文件
2. 系统自动加载，显示影院选座界面
3. 初始状态显示约21%的座位占用率

### 2. 用户注册/登录

1. 点击右上角"🔑 登录 / 注册"按钮
2. **注册新用户**:
   - 输入用户名(3-20字符，支持中英文数字)
   - 设置密码(6-50字符)
   - 确认密码
   - 可选填写邮箱
3. **登录现有用户**:
   - 输入用户名和密码
   - 系统自动验证并登录

### 3. 选座购票流程

#### 3.1 个人票购买

1. 选择"个人票"模式
2. 填写姓名和年龄信息
3. **选座方式**:
   - **手动选座**: 直接点击空座位
   - **多选模式**: 按住Ctrl键+点击多个座位
   - **自动选座**: 点击"🤖 自动选座"按钮
4. 确认选择后点击"💳 直接购票"或"📝 预订票"

#### 3.2 团体票购买

1. 选择"团体票"模式
2. 输入团体人数(2-20人)
3. 填写所有成员的姓名和年龄
4. 系统自动寻找连续座位或手动选择
5. 完成购票流程

#### 3.3 票务管理

- **查看票据**: 在"我的票据"区域查看预订和购票记录
- **取消预订**: 选择预订座位后点击"❌ 取消预订"
- **退票**: 选择已购买座位后点击"💸 退票"

### 4. 座位状态说明

| 颜色 | 状态 | 说明 |
|------|------|------|
| 🟢 绿色 | 空座位 | 可以选择的座位 |
| 🟡 黄色 | 已选座位 | 当前选中的座位 |
| 🔴 红色 | 他人已售 | 其他用户已购买的座位 |
| 🟠 橙色 | 他人预订 | 其他用户预订的座位 |
| 🔴 红色+橙边 | 我的购票 | 自己购买的座位 |
| 🟠 橙色+蓝边 | 我的预订 | 自己预订的座位 |
| ⭐ 金色发光 | 我的票(选中) | 选中的自己座位 |
| 🚫 灰色 | 年龄限制 | 因年龄限制不可选择 |

### 5. 高级功能

#### 5.1 年龄限制规则

- **少年限制**: 15岁以下不能坐前3排
- **老年限制**: 60岁以上不能坐后3排
- **实时反馈**: 输入年龄后立即显示限制座位

#### 5.2 快捷操作

- **Ctrl+点击**: 多选座位
- **清空选择**: 清除当前选中的座位
- **清空个人信息**: 一键清除所有输入信息
- **刷新票据**: 更新个人票据显示

#### 5.3 影院配置

- **座位总数**: 支持100/200/300座配置
- **固定布局**: 每排20座标准设计
- **自动适配**: 不同座位数量自动调整布局

## 🐛 遇到的问题及解决办法

### 1. 初始显示问题

**问题描述**: 系统启动时所有座位显示为绿色，未正确显示已占用座位。

**原因分析**:

- 初始化函数执行顺序错误
- `drawCinema()`在`addMockSoldSeats()`之前执行
- 导致Canvas绘制时没有座位占用数据

**解决方案**:

```javascript
// 修正后的初始化顺序
document.addEventListener('DOMContentLoaded', function () {
    initializeSeats();        // 1. 初始化座位数据
    addMockSoldSeats();       // 2. 生成模拟占用数据
    drawCinema();             // 3. 绘制Canvas显示
    refreshUserTickets();     // 4. 刷新用户票据
});
```

### 2. 座位点击检测精度问题

**问题描述**: 用户点击座位时存在偏移，只有特定区域点击有效。

**原因分析**:

- Canvas坐标系与DOM坐标系不一致
- 缺少缩放比例计算
- 响应式布局导致坐标偏移

**解决方案**:

```javascript
// 精确的坐标转换算法
function handleSeatClick(event) {
    const rect = canvas.getBoundingClientRect();
    const scaleX = canvas.width / rect.width;
    const scaleY = canvas.height / rect.height;
    
    const clickX = (event.clientX - rect.left) * scaleX;
    const clickY = (event.clientY - rect.top) * scaleY;
}
```

### 3. 用户座位状态区分问题

**问题描述**: 无法区分自己的座位和他人的座位，用户体验差。

**原因分析**:

- 缺少用户身份识别机制
- 座位状态判断逻辑不完整
- 视觉反馈不够明显

**解决方案**:

```javascript
// 用户座位特殊标识系统
if (isUserPurchased) {
    color = '#e74c3c';
    borderColor = '#f39c12';
    borderWidth = 3;
    hasUserMark = true; // 添加"我"字标记
}

// 选中状态特殊效果
if (isSelected && hasUserMark) {
    borderColor = '#ffd700'; // 金色边框
    hasGlow = true;          // 发光效果
}
```

### 4. 年龄限制UI反馈问题

**问题描述**: 年龄限制只在点击时提示，缺少视觉预警。

**原因分析**:

- 缺少实时年龄检查机制
- UI没有对限制座位进行标识
- 用户体验不够友好

**解决方案**:

```javascript
// 实时年龄限制检查
function isRestrictedByAge(seat) {
    const age = parseInt(document.getElementById('customerAge').value);
    if (age < 15 && seat.row <= 3) return true;
    if (age >= 60 && seat.row > cinemaConfig.rows - 3) return true;
    return false;
}

// UI实时反馈
customerAgeInput.addEventListener('input', function () {
    drawCinema(); // 重新绘制，显示限制座位
});
```

### 5. Canvas内容消失问题

**问题描述**: 某些操作后Canvas内容会消失或显示异常。

**原因分析**:

- Canvas绘制状态管理错误
- 缺少错误边界处理
- 异步操作导致状态混乱

**解决方案**:

```javascript
// 安全的Canvas状态管理
function drawSeat(seat, x, y, size, angle, seatLabel) {
    ctx.save(); // 保存当前状态
    try {
        // 绘制逻辑
        ctx.translate(centerX, centerY);
        ctx.rotate(rotationAngle);
        // ... 绘制代码
    } catch (error) {
        console.error('Canvas绘制错误:', error);
    } finally {
        ctx.restore(); // 无论如何都恢复状态
    }
}
```

### 6. 用户数据持久化问题

**问题描述**: 用户登录状态和票据数据在页面刷新后丢失。

**原因分析**:

- 缺少localStorage数据恢复机制
- 用户状态初始化逻辑不完整
- 数据结构设计不够健壮

**解决方案**:

```javascript
// 完整的用户状态恢复系统
function initializeUserSystem() {
    const currentUsername = localStorage.getItem('currentUser');
    if (currentUsername) {
        const users = JSON.parse(localStorage.getItem('cinemaUsers') || '{}');
        if (users[currentUsername]) {
            currentUser = users[currentUsername];
            // 恢复用户票据数据
            restoreUserTickets();
        }
    }
    updateUserUI();
}
```

### 7. 响应式布局问题

**问题描述**: 不同屏幕尺寸下布局显示异常，控制面板过窄。

**原因分析**:

- CSS Flexbox比例设置不合理
- 缺少适当的响应式断点
- 空间利用率不高

**解决方案**:

```css
/* 优化的响应式布局 */
.cinema-section {
    flex: 3;
    max-width: 1000px;
}

.control-section {
    flex: 2;
    min-width: 400px;
    max-width: 500px;
}

@media (min-width: 1600px) {
    .control-section {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 20px;
        max-width: 600px;
    }
}
```

### 8. 团体票成员信息显示错误

**问题描述**: 团体票预订/购票时，"我的票据"信息栏显示的购票人信息全部为第一个成员，而不是各座位对应的成员信息。

**原因分析**:

- `reserveTickets()`和`purchaseTickets()`函数中团体票处理逻辑有误
- 所有座位都使用第一个成员（`memberName0`）的信息
- 缺少座位与成员的正确映射关系

**解决方案**:

```javascript
// 修复团体票座位-成员信息映射
selectedSeats.forEach((seat, index) => {
    // ... 其他逻辑
    
    if (ticketType === 'individual') {
        // 个人票处理逻辑
    } else {
        // 团体票，为每个座位分配对应的成员信息
        const memberIndex = index % parseInt(document.getElementById('groupSize').value);
        const name = document.getElementById(`memberName${memberIndex}`).value.trim();
        const age = parseInt(document.getElementById(`memberAge${memberIndex}`).value);
        ticketInfo = { name: name + '(团体)', age, timestamp, seat, userId: currentUser.username };
    }
    
    // ... 后续处理
});
```

**修复效果**: 现在团体票中每个座位都显示正确的购票人姓名，不再全部显示第一个成员信息。

### 9. 已预订票付款状态转换问题

**问题描述**: 用户为已预订的票付款时，座位状态变为已购买，但"我的票据"中没有将其从"我的预订"中删除，导致重复显示。

**原因分析**:

- `purchaseTickets()`函数删除了内存中的预订记录，但未删除localStorage中的预订记录
- `currentUser.tickets`数组中仍保留`status: 'reserved'`的记录
- 数据不一致导致界面显示错误

**解决方案**:

```javascript
// 在purchaseTickets()函数中添加删除预订记录的逻辑
selectedSeats.forEach((seat, index) => {
    soldSeats.add(seat.number);
    reservedSeats.delete(seat.number);
    userTickets.reserved.delete(seat.number);

    // 从用户票据记录中删除之前的预订记录（如果存在）
    if (currentUser.tickets) {
        currentUser.tickets = currentUser.tickets.filter(t =>
            !(t.seatNumber === seat.number && t.status === 'reserved')
        );
    }

    // ... 添加新的购票记录
});
```

**修复效果**: 现在付款流程完全正确，座位从"我的预订"转移到"我的购票"，不会重复显示。

## 📚 参考资料

### 1. 技术文档

#### HTML5 Canvas

- **MDN Canvas API**: <https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API>
- **Canvas 2D Context**: <https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D>
- **Canvas坐标变换**: <https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/transform>

#### JavaScript ES6+

- **MDN JavaScript Guide**: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide>
- **ES6 Features**: <http://es6-features.org/>
- **JavaScript异步编程**: <https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous>

#### CSS3与响应式设计

- **CSS Flexbox**: <https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout>
- **CSS Grid**: <https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout>
- **响应式设计**: <https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design>

### 2. 设计参考

#### UI/UX设计

- **Material Design**: <https://material.io/design>
- **Cinema Seat Layout Standards**: 国际影院设计标准
- **Color Theory**: <https://www.interaction-design.org/literature/topics/color-theory>

#### 用户体验

- **Web用户体验设计**: Steve Krug - "Don't Make Me Think"
- **交互设计模式**: <http://ui-patterns.com/>
- **可访问性设计**: <https://www.w3.org/WAI/WCAG21/quickref/>

### 3. 开发工具与资源

#### 开发环境

- **Visual Studio Code**: <https://code.visualstudio.com/>
- **Chrome DevTools**: <https://developers.google.com/web/tools/chrome-devtools>
- **Git版本控制**: <https://git-scm.com/doc>

#### 测试与调试

- **Browser Compatibility**: <https://caniuse.com/>
- **Performance Testing**: <https://web.dev/performance/>
- **Accessibility Testing**: <https://www.w3.org/WAI/test-evaluate/>

### 4. 算法与数据结构

#### 几何算法

- **极坐标系统**: <https://en.wikipedia.org/wiki/Polar_coordinate_system>
- **2D图形变换**: <https://en.wikipedia.org/wiki/Transformation_matrix>
- **碰撞检测算法**: <https://developer.mozilla.org/en-US/docs/Games/Techniques/2D_collision_detection>

#### 搜索与排序

- **距离算法**: 曼哈顿距离、欧几里得距离
- **贪心算法**: 用于自动选座优化
- **动态规划**: 连续座位分配算法

### 5. 项目管理与最佳实践

#### 代码规范

- **JavaScript Style Guide**: <https://github.com/airbnb/javascript>
- **HTML/CSS Best Practices**: <https://github.com/hail2u/html-best-practices>
- **Git Commit Guidelines**: <https://www.conventionalcommits.org/>

#### 安全性

- **Frontend Security**: <https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html>
- **Local Storage Security**: <https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API>

## 📈 项目总结

### 1. 技术成果

- **完整的前端应用**: 实现了从用户注册到购票完成的完整流程
- **高性能Canvas渲染**: 支持300座大型影院的流畅显示
- **智能交互设计**: 8种座位状态、实时年龄限制、多种选座模式
- **数据持久化**: localStorage实现的完整用户数据管理
- **响应式设计**: 支持多种屏幕尺寸的自适应布局

### 2. 用户体验亮点

- **直观的视觉设计**: 弧形座位布局模拟真实影院
- **智能提示系统**: 详细的操作说明和错误提示
- **多样化选座**: 手动选座、自动选座、多选模式
- **实时反馈**: 年龄限制、座位状态即时更新
- **便捷操作**: 一键清空、快捷键支持

### 3. 技术创新点

- **极坐标座位布局**: 创新的弧形影院设计算法
- **Canvas状态管理**: 高效的图形渲染和状态同步
- **智能选座算法**: 基于距离优化的自动推荐
- **用户座位识别**: 特殊的视觉标识和交互方式
- **实时限制检查**: 动态年龄限制UI反馈

### 4. 项目价值

- **教育价值**: 综合运用了HTML5、CSS3、JavaScript等前端技术
- **实用价值**: 可直接应用于真实的影院选座场景
- **扩展价值**: 代码结构清晰，易于功能扩展和维护
- **学习价值**: 涵盖了前端开发的多个核心技术点

### 5. 后续改进方向

- **后端集成**: 连接真实的数据库和服务器
- **移动端优化**: 进一步优化移动设备体验
- **多影院支持**: 扩展为多影院、多场次系统
- **支付集成**: 集成真实的支付系统
- **实时通信**: 添加WebSocket实现实时座位更新

---

**项目完成时间**: 2025年7月21日  
**文档版本**: v1.3.1-stable  
**总开发时长**: 约30天  
**代码行数**: 2400+ 行  
**功能完成度**: 99%  

这个项目成功实现了一个功能完整、用户体验良好的电影院选座系统，在技术实现和用户体验方面都达到了预期目标。
