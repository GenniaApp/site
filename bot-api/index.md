# Bot 开放平台

本文介绍 Gennia 的 `socket API`。以便开发者造出相应的游戏机器人。

## 接收 / `On`

### `reject_join`

加入房间申请被拒绝。

含有两个参数：`title, message`，为拒绝理由（Gennia 只设计了一个玩家队列，不允许在游戏开始后加入房间）。

### `server_info`

服务器信息，含两个参数：`name, version`。分别为服务器名，版本号。通过发送 `query_server_info` 获取。

### `connect`

连接到服务器房间。

### `set_player_id`

含一个参数，为在加入房间后 Gennia 向你提供的 `playerId`。供断线重连使用。（**该功能未完善**）

### `room_message`

接收系统/玩家发布的聊天信息。

### `game_started`

含一个参数：`playerColor`，为你的颜色代号 $(0\sim 7)$ 。

### `game_update`

游戏更新。每个游戏刻都会下传。参数列表：

```js
gameMap, width, height, turn, leaderBoard
```

#### `width` 与 `height`

与原含义颠倒。`width` 代表竖向格子数，`height` 代表横向格子数。

#### `gameMap` 数据结构

`String` 化的二维 `Array`。使用时需用 `JSON.parse()` 方式还原。

#### `leaderBoard` 数据结构

一维 `Array`。拥有如下属性：

```js
{
 color,
 username,
 land,
 army
}
```

### `attack_failure`

你发送的上一条 `attack` 请求不满足要求。

### `game_over`

含一个参数：`player`，表示你被该玩家攻占。通常需要与 `leave_game` 配合使用。

`player` 为 `player Class`。

一个 `player Class` 含有以下属性：

```js
{
 id: this.id,
 socket_id: this.socket_id,
 username: this.username,
 color: this.color,
 isRoomHost: this.isRoomHost,
 forceStart: this.forceStart,
 isDead: this.isDead,
 operatedTurn: this.operatedTurn
}
```

### `game_ended`

整场游戏结束。含一个参数：`player`，表示最后赢家。

## 发送 / `Emit`

### `query_server_info`

向 Gennia 请求服务器信息。服务器会返回 `server_info`。

### `set_username`

这个是你愿意加入游戏的标志。

发送一个参数 `username`（可以与他人重复）。发送后您会接收到 `set_player_id`。就是您的 `playerId`。

### `player_message`

发送一个参数 `message`。

> **注意**：您只能在游戏**开始前**发送聊天请求

### `attack`

需要包含三个参数：

```js
{
 from: Point,
 to: Point,
 half: Boolean
}
```

其中 `Point` 类含两个属性：`x, y`。

`half` 代表你是否希望进行半数攻击。

> **注意**：您 $1$ 游戏刻内只能进行一次攻击。后面几次 Gennia 不会接收，而是返回 `attack_failure`。
