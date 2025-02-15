# 接口一览

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="cpp" label="C++" default>

```cpp
    // 通用接口
    // 发送信息、接受信息，注意收消息时无消息则返回nullopt
    virtual std::future<bool> SendTextMessage(int32_t toPlayerID, std::string) = 0;
    virtual std::future<bool> SendBinaryMessage(int32_t toPlayerID, std::string) = 0;
    [[nodiscard]] virtual bool HaveMessage() = 0;
    [[nodiscard]] virtual std::pair<int32_t, std::string> GetMessage() = 0;

    // 获取游戏目前所进行的帧数
    [[nodiscard]] virtual int32_t GetFrameCount() const = 0;
    // 等待下一帧
    virtual bool Wait() = 0;
    virtual std::future<bool> EndAllAction() = 0;
    [[nodiscard]] virtual std::vector<std::shared_ptr<const THUAI7::Ship>> GetShips() const = 0;
    [[nodiscard]] virtual std::vector<std::shared_ptr<const THUAI7::Ship>> GetEnemyShips() const = 0;
    [[nodiscard]] virtual std::vector<std::shared_ptr<const THUAI7::Bullet>> GetBullets() const = 0;
    [[nodiscard]] virtual std::vector<std::vector<THUAI7::PlaceType>> GetFullMap() const = 0;
    [[nodiscard]] virtual std::shared_ptr<const THUAI7::GameInfo> GetGameInfo() const = 0;
    [[nodiscard]] virtual THUAI7::PlaceType GetPlaceType(int32_t cellX, int32_t cellY) const = 0;
    [[nodiscard]] virtual int32_t GetConstructionHp(int32_t cellX, int32_t cellY) const = 0;
    [[nodiscard]] virtual int32_t GetWormholeHp(int32_t cellX, int32_t cellY) const = 0;
    [[nodiscard]] virtual int32_t GetResourceState(int32_t cellX, int32_t cellY) const = 0;
    [[nodiscard]] virtual int32_t GetHomeHp() const = 0;
    [[nodiscard]] virtual int32_t GetEnergy() const = 0;
    [[nodiscard]] virtual int32_t GetScore() const = 0;
    [[nodiscard]] virtual std::vector<int64_t> GetPlayerGUIDs() const = 0;

    // 船只
    // 指挥本角色进行移动，`timeInMilliseconds` 为移动时间，单位为毫秒；`angleInRadian` 表示移动的方向，单位是弧度，使用极坐标——竖直向下方向为 x 轴，水平向右方向为 y 轴
    virtual std::future<bool> Move(int64_t timeInMilliseconds, double angleInRadian) = 0;
    // 向特定方向移动
    virtual std::future<bool> MoveRight(int64_t timeInMilliseconds) = 0;
    virtual std::future<bool> MoveUp(int64_t timeInMilliseconds) = 0;
    virtual std::future<bool> MoveLeft(int64_t timeInMilliseconds) = 0;
    virtual std::future<bool> MoveDown(int64_t timeInMilliseconds) = 0;
    virtual std::future<bool> Attack(double angleInRadian) = 0;
    virtual std::future<bool> Recover(int64_t recover) = 0;
    virtual std::future<bool> Produce() = 0;
    virtual std::future<bool> Rebuild(THUAI7::ConstructionType constructionType) = 0;
    virtual std::future<bool> Construct(THUAI7::ConstructionType constructionType) = 0;
    virtual std::shared_ptr<const THUAI7::Ship> GetSelfInfo() const = 0;
    virtual bool HaveView(int32_t targetX, int32_t targetY) const = 0;

    // 大本营
    [[nodiscard]] virtual std::shared_ptr<const THUAI7::Team> GetSelfInfo() const = 0;
    virtual std::future<bool> InstallModule(int32_t playerID, THUAI7::ModuleType moduletype) = 0;
    virtual std::future<bool> Recycle(int32_t playerID) = 0;
    virtual std::future<bool> BuildShip(THUAI7::ShipType ShipType, int32_t birthIndex) = 0;

    /*****选手可能用的辅助函数*****/

    // 获取指定格子中心的坐标
    [[nodiscard]] static inline int32_t CellToGrid(int32_t cell) noexcept
    {
        return cell * numOfGridPerCell + numOfGridPerCell / 2;
    }

    // 获取指定坐标点所位于的格子的 X 序号
    [[nodiscard]] static inline int32_t GridToCell(int32_t grid) noexcept
    {
        return grid / numOfGridPerCell;
    }

    // 用于DEBUG的输出函数，选手仅在开启Debug模式的情况下可以使用

    virtual void Print(std::string str) const = 0;
    virtual void PrintShip() const = 0;
    virtual void PrintTeam() const = 0;
    virtual void PrintSelfInfo() const = 0;
```

</TabItem>
<TabItem value="python" label="Python">

```python
    """
    选手可执行的操作，应当保证所有函数的返回值都应当为 `asyncio.Future`，例如下面的移动函数：\n
    指挥本角色进行移动：
    - `timeInMilliseconds` 为移动时间，单位为毫秒
    - `angleInRadian` 表示移动的方向，单位是弧度，使用极坐标——竖直向下方向为 x 轴，水平向右方向为 y 轴\n
    发送信息、接受信息，注意收消息时无消息则返回 `nullopt`
    """
    
    # 通用接口
    def SendMessage(self, toPlayerID: int, message: Union[str, bytes]) -> Future[bool]:
    def HaveMessage(self) -> bool:
    def GetMessage(self) -> Tuple[int, str]:

    # 获取游戏目前所进行的帧数
    def GetFrameCount(self) -> int:
    def Wait(self) -> Future[bool]:
    def EndAllAction(self) -> Future[bool]:
    
    def GetShips(self) -> List[THUAI7.Ship]:
    def GetEnemyShips(self) -> List[THUAI7.Ship]:
    def GetBullets(self) -> List[THUAI7.Bullet]:
    def GetFullMap(self) -> List[List[THUAI7.PlaceType]]:
    def GetGameInfo(self) -> THUAI7.GameInfo:
    def GetPlaceType(self, cellX: int, cellY: int) -> THUAI7.PlaceType:
    def GetConstructionHp(self, cellX: int, cellY: int) -> int:
    def GetWormholeHp(self, cellX: int, cellY: int) -> int:
    def GetResourceState(self, cellX: int, cellY: int) -> int:
    def GetHomeHp(self) -> int:
    def GetEnergy(self) -> int:
    def GetScore(self) -> int:
    def GetPlayerGUIDs(self) -> List[int]:

    def Print(self, cont: str) -> None:
    def PrintShip(self) -> None:
    def PrintTeam(self) -> None:
    def PrintSelfInfo(self) -> None:
    def GetSelfInfo(self) -> Union[THUAI7.Ship, THUAI7.Team]:

    # 船只接口
    def Move(self, timeInMilliseconds: int, angleInRadian: float) -> Future[bool]:
    def MoveRight(self, timeInMilliseconds: int) -> Future[bool]:
    def MoveUp(self, timeInMilliseconds: int) -> Future[bool]:
    def MoveLeft(self, timeInMilliseconds: int) -> Future[bool]:
    def MoveDown(self, timeInMilliseconds: int) -> Future[bool]:
    def Attack(self, angleInRadian: float) -> Future[bool]:
    def Recover(self) -> Future[bool]:
    def Produce(self) -> Future[bool]:
    def Rebuild(self, constructionType: THUAI7.ConstructionType) -> Future[bool]:
    def Construct(self, constructionType: THUAI7.ConstructionType) -> Future[bool]:
    def GetSelfInfo(self) -> THUAI7.Ship:
    def HaveView(self, gridX: int, gridY: int) -> bool:

    # 大本营接口
    def GetSelfInfo(self) -> THUAI7.Team:
    def InstallModule(
        self, playerID: int, moduleType: THUAI7.ModuleType
    ) -> Future[bool]:
    def Recycle(self, playerID: int) -> Future[bool]:
    def BuildShip(self, shipType: THUAI7.ShipType) -> Future[bool]:
```

</TabItem>
</Tabs>
