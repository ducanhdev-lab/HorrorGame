# HorrorGame

Game kinh dị góc nhìn thứ nhất (FPS horror) xây dựng trên **Unity 6**, lấy cảm hứng từ kinh nghiệm sống sót trong không gian tối: khám phá, tương tác môi trường, tránh hoặc đối đầu quái vật, và giữ mạng cho đến khi hoàn thành mục tiêu trong map.

---

## 1. Đây là game gì?

**HorrorGame** là game **kinh dị – sinh tồn góc nhìn thứ nhất** (first-person horror). Người chơi điều khiển nhân vật trong môi trường u ám, dùng đèn pin, vũ khí và các điểm ẩn nấp để sống sót trước kẻ thù AI. Không khí game được củng cố bằng âm thanh (thở, tiếng động), ánh sáng nhấp nháy và vật thể rung lắc khi người chơi đến gần vùng “horror”.

Hệ thống lõi nằm trong namespace `AdvancedHorrorFPS`, với các scene chính:

| Scene | Mô tả ngắn |
|-------|------------|
| `Scene_MainMenu` | Menu, New Game / Continue, loading |
| `Interactions` | Scene gameplay chính (puzzle, tương tác, tiến trình) |
| `Enemy_AI` | AI quái (NavMesh, hành vi đuổi/tấn công) |

---

## 2. Bạn làm gì?

Trong game, người chơi thường sẽ:

- **Di chuyển & quan sát** — FPS: nhìn xung quanh, sprint, nhảy, cúi (tùy cấu hình trong `AdvancedGameManager`).
- **Tương tác môi trường** — Mở cửa, rương, ngăn kéo; nhặt **chìa khóa**, đọc **ghi chú**, kéo **thang**, đẩy **hộp**.
- **Quản lý trang bị** — Đèn pin (kể cả chế độ **tia UV xanh** để tấn công quái), súng, gậy baseball; **inventory** (Tab) khi bật trong manager.
- **Ẩn nấp** — Chui vào tủ / điểm hide; camera chuyển sang góc nhìn ẩn nấp.
- **Đối đầu hoặc trốn quái** — Enemy AI (NavMesh): tuần tra, đuổi, tấn công. Khi bị bắt có thể **spam click để thoát** (nếu bật `canPlayerEscapeByClickEnough`) hoặc chết ngay tùy `GameType`.
- **Lưu tiến độ** — Đi qua **save point** (`SaverScript`): lưu vị trí checkpoint và inventory qua `PlayerPrefs`.
- **Sống sót** — Theo dõi **máu** / stamina; thua khi hết máu hoặc bị quái hạ gục (tùy chế độ game).

**Điều khiển mặc định (PC):** WASD di chuyển, chuột nhìn, `E` tương tác, `F` đèn pin, Shift sprint, Space nhảy, Ctrl cúi. Hỗ trợ **mobile** (joystick + touchpad) khi `ControllerType` = Mobile.

---

## 3. Có gì technical đáng chú ý?

| Hạng mục | Chi tiết |
|----------|----------|
| **Engine** | Unity **6000.3.10f1** (Unity 6) |
| **Input** | [Unity Input System] — `InputAction` cấu hình trong `AdvancedGameManager`, tách PC / Mobile qua `AdvancedFPSInputManager` |
| **AI** | `DemonScript` + **NavMeshAgent**: Idle → Patrol → Chase → Attack; loại enemy Crawling / Walking / Random; scale theo **Difficulty** |
| **Horror ambience** | `HorrorDetector` — volume âm thanh theo khoảng cách tới `HorrorItem`, kích hoạt `ShakableObject` / `FlickableObject` quanh người chơi |
| **Kiến trúc** | Singleton managers (`AdvancedGameManager`, `GameCanvas`, `HeroPlayerScript`, `InventoryManager`); gameplay bật/tắt feature qua Inspector (jump, crouch, inventory, UV light, v.v.) |
| **Lưu game** | `PlayerPrefs` + checkpoint index + `InventoryManager.SaveInventory` / `LoadInventory` |
| **UI / flow** | Async load scene + thanh progress (`MainMenuCanvas`); pause, game over, UnityEvent `gameOverEventToInvoke` |
| **Packages** | AI Navigation 2.x, Input System 1.18, uGUI 2.0 |

**Cấu trúc code chính:**

```
Assets/_Game/
├── Scenes/          # MainMenu, Interactions, Enemy_AI
├── Scripts/         # Script gameplay (player, AI, UI, items, save, …)
└── Prefabs/         # Player, enemies, interactables
```

---

## 4. Playable / Video

