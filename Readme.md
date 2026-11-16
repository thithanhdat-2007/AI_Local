# CẨM NANG TOÀN TẬP: CÀI ĐẶT AI LOCAL & SẢN XUẤT ASSET THỦY MẶC
**Dự án:** Vạn Kiếm Quy Tông (Survivor-like Tu Tiên – Unity 6 URP 2D)  
**Phong cách nghệ thuật:** Thủy Mặc Tương Phản Cao (High-Contrast Dark Sumi-e)  
**Cấu hình mục tiêu:** GPU RTX 5060 (8GB VRAM) | 16GB RAM | Độ phân giải chuẩn $1920 \times 1080$

---

## PHẦN I: THIẾT LẬP HẠ TẦNG AI LOCAL & TỰ ĐỘNG HÓA

### 1. Tối ưu Windows 11 (Chống tràn 16GB RAM khi chạy song song Unity + AI)
1. Mở thanh tìm kiếm Windows, gõ **View advanced system settings** $\rightarrow$ mở.
2. Tại tab **Advanced** $\rightarrow$ mục **Performance** $\rightarrow$ bấm **Settings...**
3. Chuyển sang tab **Advanced** $\rightarrow$ mục **Virtual memory** $\rightarrow$ bấm **Change...**
4. Bỏ tích ô **Automatically manage paging file size for all drives**.
5. Chọn ổ đĩa cài dữ liệu $\rightarrow$ chọn **Custom size** $\rightarrow$ nhập:
   * **Initial size:** `16384`
   * **Maximum size:** `24576`
6. Bấm **Set** $\rightarrow$ **OK** $\rightarrow$ Khởi động lại máy tính.

### 2. Cài đặt Krita & Plugin Krita AI Diffusion
1. **Cài Krita:** Tải và cài đặt bản **Krita Windows 64-bit** từ trang chủ chính thức.
2. **Tải Plugin:** Truy cập GitHub **`Acly/krita-ai-diffusion`** $\rightarrow$ mục **Releases** $\rightarrow$ tải file `krita_ai_diffusion-x.x.x.zip` (giữ nguyên file nén `.zip`).
3. **Kích hoạt Plugin:**
   * Mở Krita $\rightarrow$ thanh menu chọn **Tools** $\rightarrow$ **Scripts** $\rightarrow$ **Import Python Plugin from File...** $\rightarrow$ trỏ tới file `.zip` vừa tải $\rightarrow$ chọn **Yes** $\rightarrow$ tắt Krita và mở lại.
   * Vào **Settings** $\rightarrow$ **Dockers** $\rightarrow$ tích chọn **AI Image Generation**.

### 3. Cài đặt Lõi Xử Lý & Nạp Model Thủy Mặc
1. Trên bảng **AI Image Generation**, bấm biểu tượng bánh răng (**Configure**).
2. Tab **Connection**: Chọn **Local Managed Server** $\rightarrow$ đặt **Server Path** tại thư mục không dấu (ví dụ: `D:\TuTien_AI`).
3. Tích chọn gói **SDXL** (tối ưu nhất cho 8GB VRAM) $\rightarrow$ bấm **Install** và chờ tải hoàn tất.
4. **Tải Model & LoRA từ Civitai / Hugging Face:**
   * **Model gốc (Checkpoint - ~6.5GB):** Tải **Illustrious-XL** hoặc **Animagine XL 3.1** (đuôi `.safetensors`).
   * **LoRA bổ trợ (~100-200MB):** Tải 1 LoRA phong cách thủy mặc (từ khóa `sumi-e` hoặc `ink wash`, hệ `SDXL`).
5. Quay lại **Configure** $\rightarrow$ tab **Styles** $\rightarrow$ bấm biểu tượng thư mục cạnh **Model Checkpoint** và **LoRA** để chép các file vừa tải vào đúng ngăn $\rightarrow$ bấm nút **Refresh** (mũi tên xoay vòng).

### 4. Quy trình 4 bước vẽ & xuất Asset trong Krita
1. **Tạo khung vẽ:** `File -> New` kích thước $1024 \times 1024$ px (hoặc $512 \times 512$ px). Ở bảng **Layers**, tắt biểu tượng con mắt ở lớp **Background** để lộ nền caro trong suốt.
2. **Vẽ phác thảo (Sketch):** Tạo Layer mới, dùng cọ đen vẽ nét phác hình dáng thô (giúp AI khóa đúng bố cục, góc nghiêng).
3. **Chạy AI:** Dán Prompt tiếng Anh tương ứng ở các phần dưới, chỉnh thanh **Strength** ở mức **70% – 80%** $\rightarrow$ bấm **Generate** $\rightarrow$ chọn ảnh ưng ý nhất và bấm **Apply**.
4. **Thu nhỏ & Xuất file:** Vào `Image -> Scale Image To New Size...` để đưa về đúng kích thước quy định của từng tuần $\rightarrow$ `File -> Export...` lưu dưới định dạng `.png`.

### 5. Tách nền hàng loạt miễn phí (Chạy Local)
Thay vì tách nền thủ công từng tấm, hãy gom tất cả ảnh AI xuất ra vào 1 thư mục `Raw_Assets/` và dùng một trong hai công cụ sau:
* **Cách 1 – Dùng dòng lệnh `rembg` (Nhanh nhất):**
  ```bash
  # Cài đặt 1 lần duy nhất
  pip install "rembg[cli]"
  
  # Chạy tách nền hàng loạt với model nét căng cho game 2D
  rembg p -m isnet-general-use D:\Raw_Assets D:\Clean_Assets
  ```
* **Cách 2 – Dùng phần mềm `chaiNNer` (Giao diện kéo thả Node):**
  Nối chuỗi: `Load Image Iterator` $\rightarrow$ `Remove Background` $\rightarrow$ `Resize` (ép về $512 \times 512$ hoặc $256 \times 256$) $\rightarrow$ `Save Image`.

---

## QUY CHUẨN PROMPT CHUNG (DÙNG CHO TOÀN BỘ DỰ ÁN)

* **Negative Prompt (Dán vào ô Negative Prompt cho mọi lần tạo ảnh):**
  ```text
  3d render, realistic, photorealistic, blurry, low quality, watermark, signature, text, english letters, chinese characters, multiple views, character sheet, grid, split screen, cropped, out of frame, messy background, grey background, gradient background, drop shadow
  ```

---

## PHẦN II: TUẦN 3 – TẠO HÌNH NHÂN VẬT (4 LƯU PHÁI), 5 YÊU THÚ & 2 BOSS

### Bước 3.1 – Tạo 4 Lưu Phái Nhân Vật ($512 \times 512$ px)
* **Thư mục lưu trữ:** `Assets/Art/Characters/`
* **Quy tắc thiết kế:** Tách rời thân và vũ khí (tay không hoặc cầm vật phụ trợ), nền trắng tinh để dễ tách nền, tông đen trắng thủy mặc kèm điểm nhấn phát sáng đặc trưng.

1. **Kiếm Tu (`Char_KiemTu.png`)** – *Đạo sĩ trẻ áo trắng viền xanh ngọc, tay không:*
   ```text
   solo, full body, young taoist swordsman cultivator in flowing white and black ink wash robes with glowing cyan trim, glowing cyan eyes, empty hands in dynamic martial arts idle stance, no weapon in hand, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game character sprite
   ```

2. **Thể Tu (`Char_TheTu.png`)** – *Võ tăng/Lực sĩ cơ bắp quấn băng vải, hào quang vàng đất:*
   ```text
   solo, full body, muscular martial monk body cultivator with wrapped cloth bandages on fists and torso, dark ink wash trousers, glowing earthy gold aura and eyes, powerful grounded stance, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game character sprite
   ```

3. **Phù Tu (`Char_PhuTu.png`)** – *Đạo trưởng đội mũ quan, tay cầm phất trần, hào quang xanh lam:*
   ```text
   solo, full body, taoist master talisman cultivator wearing traditional scholar crown hat and flowing ink wash robes, holding a horsetail whisk, glowing azure blue aura and eyes, mystic pose, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game character sprite
   ```

4. **Dược Tu (`Char_DuocTu.png`)** – *Y sư đeo hồ lô sau lưng, hào quang xanh lục bảo:*
   ```text
   solo, full body, ancient eastern alchemist healer cultivator wearing scholar robes with a large medicine gourd tied on back, glowing emerald green aura and eyes, calm agile pose, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game character sprite
   ```

---

### Bước 3.2 – Tạo 5 Loại Yêu Thú Thủy Mặc ($512 \times 512$ px)
* **Thư mục lưu trữ:** `Assets/Art/Enemies/`
* **Quy tắc thiết kế:** Đồng bộ tông đen mực tàu và khói xám, đổi màu đôi mắt/hoa văn theo hệ Ngũ Hành.

1. **Hắc Lang Yêu – Hệ Mộc (`Enemy_HacLang.png`)** – *Sói ma khói đen, mắt xanh lục:*
   ```text
   solo, demonic black shadow wolf monster made of swirling dark ink smoke, fiercely glowing emerald green eyes and forest spirit markings, aggressive pouncing pose, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game enemy sprite
   ```

2. **Huyết Tự Dơi – Hệ Hỏa (`Enemy_HuyetDoi.png`)** – *Dơi quỷ cánh rách, bụng phát sáng đỏ rực sắp nổ:*
   ```text
   solo, monstrous demonic bat with tattered black ink wings, bloated belly glowing bright crimson red like molten fire about to explode, glowing red eyes, flying pose, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game enemy sprite
   ```

3. **Độc Giác Xà – Hệ Thủy (`Enemy_DocGiacXa.png`)** – *Rắn độc một sừng, vảy ánh xanh lam:*
   ```text
   solo, giant venomous single-horned serpent monster coiled and ready to strike, dark ink black scales with glowing sapphire blue bioluminescent patterns and eyes, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game enemy sprite
   ```

4. **Khô Cốt Tế Tư – Hệ Thổ (`Enemy_KhoCot.png`)** – *Pháp sư xương khô cầm trượng, ánh vàng nâu:*
   ```text
   solo, undead skeleton shaman sorcerer wearing tattered black ink robes, holding a gnarled bone staff, glowing amber brown earth energy and eyes, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game enemy sprite
   ```

5. **Thiết Giáp Hùng – Tinh Anh / Hệ Kim (`Enemy_ThietGiapHung.png`)** – *Gấu khổng lồ mặc giáp sắt cổ, hào quang tím:*
   ```text
   solo, colossal armored bear monster wearing heavy ancient dark iron plates, glowing violet purple metallic aura and eyes, intimidating towering stance, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game elite enemy sprite
   ```

---

### Bước 3.3 – Tạo 2 Boss Yêu Vương ($1024 \times 1024$ px)
* **Thư mục lưu trữ:** `Assets/Art/Enemies/`

1. **Boss Phút 5: Huyết Nha Yêu Vương (`Boss_HuyetNha.png`)** – *Quái thú lợn rừng khổng lồ nanh đỏ rực:*
   ```text
   solo, colossal demonic wild boar demon king, massive black ink fur body covered in battle scars, huge razor-sharp tusks glowing bright crimson red, ferocious roaring pose, billowing dark ink smoke, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game boss sprite
   ```

2. **Boss Phút 10: Cửu U Lôi Giao (`Boss_CuuULoiGiao.png`)** – *Giao long hắc ám uốn lượn quấn quanh tia sét tím:*
   ```text
   solo, colossal abyssal black ink flood dragon coiling in the air, surrounded by crackling glowing purple lightning bolts, fierce horns and glowing violet eyes, majestic terrifying boss pose, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, clean edges, 2d game boss sprite
   ```

---

### Bước 3.4 – Chuẩn hóa thông số Import trong Unity 6
1. Bôi đen toàn bộ 11 ảnh nhân vật, yêu thú và boss trong cửa sổ Project (`Assets/Art/Characters` và `Assets/Art/Enemies`).
2. Nhìn sang cửa sổ **Inspector** bên phải, thiết lập:
   * **Texture Type:** `Sprite (2D and UI)`
   * **Sprite Mode:** `Single`
   * **Pivot:** Chuyển sang **`Bottom`** (đặt tâm tọa độ ngay dưới chân để nhân vật đứng chuẩn trên mặt đất và xoay lật trái/phải không bị lệch vị trí).
   * **Filter Mode:** `Bilinear` | **Compression:** `Normal Quality`.
3. Bấm **Apply**. Khi kéo vào Prefab, gắn vật liệu **`Unlit_Sprite_Mat`** vào thành phần `Sprite Renderer` để hình ảnh sắc nét, không bị tối bởi ánh sáng môi trường.

---

## PHẦN III: TUẦN 4 – BỘ ICON PHÁP BẢO, ĐAN DƯỢC, ĐẠO QUẢ & VẬT PHẨM RƠI

### Bước 4.1 – Tạo hình 6 Pháp Bảo ($256 \times 256$ px)
* **Thư mục lưu trữ:** `Assets/Art/Icons/`
* **Lưu ý:** Vũ khí nên vẽ thẳng đứng (hướng mũi lên trên) để khi lập trình xoay góc trong Unity khớp với trục tọa độ chuẩn.

1. **Cự Kiếm (`Weapon_CuKiem.png`):**
   ```text
   vertical heavy black iron broadsword carved with glowing golden ancient runes, massive blade, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game weapon icon, centered, clean edges
   ```

2. **Hộ Thể Kim Chung (`Weapon_KimChung.png`):**
   ```text
   ancient bronze Taoist temple bell engraved with glowing golden Bagua trigrams, mystical protective aura, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game weapon icon, centered, clean edges
   ```

3. **Huyết Đao (`Weapon_HuyetDao.png`):**
   ```text
   vertical curved demonic saber blade dripping with glowing crimson blood mist, dark ink steel, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game weapon icon, centered, clean edges
   ```

4. **Liên Hoa Đăng (`Weapon_LienHoaDang.png`):**
   ```text
   ornate ancient lotus-shaped palace lantern burning with a glowing cyan-white spiritual soul flame, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game weapon icon, centered, clean edges
   ```

5. **Thanh Trúc Phi Kiếm (`Weapon_ThanhTrucKiem.png`):**
   ```text
   vertical slender agile flying sword crafted from glowing emerald green jade bamboo, sharp ethereal edge, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game weapon icon, centered, clean edges
   ```

6. **Huyền Băng Phù (`Weapon_HuyenBangPhu.png`):**
   ```text
   vertical ancient Taoist paper talisman infused with glowing icy sapphire blue frost runes and ice crystals, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game weapon icon, centered, clean edges
   ```

---

### Bước 4.2 – Tạo bộ Icon Phường Thị & Thẻ Bài ($256 \times 256$ px)
* **Thư mục lưu trữ:** `Assets/Art/Icons/`

#### A. 6 Icon Đan Dược (Bình hồ lô & Viên đan phát sáng)
* **Prompt mẫu chung (Thay đổi màu sắc/chi tiết trong ngoặc vuông cho 6 loại):**
  ```text
  ancient chinese porcelain medicine gourd bottle next to a floating glowing [crimson red / emerald green / golden / azure blue / violet / pure white] elixir pill with swirling spiritual mist, traditional sumi-e ink wash painting style, high contrast, isolated on pure white background, 2d game item icon, centered
  ```
  *Gợi ý đặt tên:* `Item_Dan_HoiHuyet.png` (Đỏ), `Item_Dan_TuKhi.png` (Xanh ngọc), `Item_Dan_BaoNguyen.png` (Vàng cam), `Item_Dan_TayTuy.png` (Xanh lam), `Item_Dan_HoTam.png` (Tím), `Item_Dan_PhiThang.png` (Trắng sáng).

#### B. 6 Icon Đạo Quả (Trái cây tiên giới tỏa hào quang huyền bí)
* **Prompt mẫu chung (Thay đổi loại quả & màu hào quang cho 6 loại):**
  ```text
  mythical immortal heavenly fruit [glowing golden peach / fiery red dragon fruit / icy blue spirit ginseng fruit / purple lightning berry / emerald jade pear / yin-yang black and white fruit] radiating mystical aura and ink smoke, traditional chinese sumi-e ink painting style, high contrast, isolated on pure white background, 2d game icon, centered
  ```
  *Gợi ý đặt tên:* `Fruit_KiemTam.png`, `Fruit_BatDiet.png`, `Fruit_LoiDinh.png`, `Fruit_HonDon.png`, `Fruit_TruongSinh.png`, `Fruit_ThonPhe.png`.

#### C. 5 Icon Nhãn Lưu Phái (Biểu tượng tròn/ấn triện cổ)
* **Prompt mẫu chung (Thay biểu tượng trung tâm cho 5 nhãn):**
  ```text
  minimalist circular chinese calligraphy seal icon featuring a [crossed flying swords (Kiếm Đạo) / glowing fist (Thể Đạo) / mystic talisman scroll (Phù Đạo) / alchemy gourd (Đan Đạo) / demonic skull flame (Ma Đạo)], black ink brush stroke on pure white background, high contrast sumi-e style, 2d game UI badge
  ```
  *Gợi ý đặt tên:* `Tag_KiemDao.png`, `Tag_TheDao.png`, `Tag_PhuDao.png`, `Tag_DanDao.png`, `Tag_MaDao.png`.

---

### Bước 4.3 – Tạo bộ Vật Phẩm Rơi & Cảnh Báo trên Sân ($128 \times 128$ px)
* **Thư mục lưu trữ:** `Assets/Art/Icons/` (hoặc `Assets/Art/Environment/` cho dấu cảnh báo).

1. **Viên Linh Khí – XP (`Pickup_LinhKhi.png`):**
   ```text
   small glowing cyan-blue spiritual energy orb crystal radiating soft light, minimalist black ink outline, sumi-e style, isolated on pure white background, 2d game pickup icon, centered
   ```

2. **Viên Linh Thạch – Tiền tệ (`Pickup_LinhThach.png`):**
   ```text
   faceted glowing golden spirit stone gem crystal with sharp edges, minimalist black ink outline, sumi-e style, isolated on pure white background, 2d game currency icon, centered
   ```

3. **Rương Bảo Vật (`Pickup_RuongBau.png`):**
   ```text
   ancient dark wooden Chinese treasure chest reinforced with glowing golden trim and lock, black ink wash texture, sumi-e style, isolated on pure white background, 2d game icon, centered
   ```

4. **Dấu X Đỏ Cảnh Báo (`VFX_CanhBao_X.png`):**
   ```text
   bold aggressive hand-painted calligraphy brush stroke X mark in glowing crimson red ink, splatter edges, sumi-e style, isolated on pure white background, 2d game warning marker, centered
   ```

---

## PHẦN IV: TUẦN 5 – BỘ KHUNG GIAO DIỆN THỦY MẶC (UI KIT) & KỸ THUẬT 9-SLICE

### Bước 5.1 – Tạo các thành phần UI nền tảng
* **Thư mục lưu trữ:** `Assets/Art/UI_Kit/`

1. **Khung Bảng Lớn – Panel ($512 \times 512$ px | `UI_Panel_Scroll.png`):**
   ```text
   rectangular ancient dark charcoal-grey parchment scroll UI window frame with subtle glowing muted gold ink brush borders and corner cloud motifs, empty dark center, top-down flat 2d game UI panel, isolated on pure white background, clean straight edges
   ```

2. **Thẻ Bài Đứng – Card Frame ($256 \times 512$ px | `UI_Card_Frame.png`):**
   ```text
   vertical rectangular card frame for 2d game UI, dark sumi-e ink wash background texture, ornate silver-white calligraphy brush border, empty center space for icon and text, flat 2d game UI asset, isolated on pure white background
   ```

3. **5 Viền Phẩm Cấp – Rarity Borders ($256 \times 256$ px):**
   * **Prompt mẫu (Thay màu phát sáng cho 5 cấp: Phàm = Trắng, Linh = Xanh Lam, Địa = Tím, Thiên = Vàng Cam, Tiên = Đỏ Rực):**
     ```text
     square 2d game item slot border frame with glowing [pure white / azure blue / violet purple / golden orange / blazing crimson red] ink brush energy aura, completely empty transparent-ready white center, ornate oriental corner flourishes, flat 2d game UI border, isolated on pure white background
     ```
   * *Tên file:* `Border_1_Pham.png`, `Border_2_Linh.png`, `Border_3_Dia.png`, `Border_4_Thien.png`, `Border_5_Tien.png`.

4. **Thanh Trạng Thái & Biểu Tượng Độn Thuật:**
   * **Khung thanh trạng thái ($512 \times 64$ px | `UI_Bar_Frame.png`):**
     ```text
     horizontal elongated rectangular 2d game health bar frame, dark ink iron border with sharp calligraphy brush ends, empty white interior, flat 2d game UI asset, isolated on pure white background
     ```
   * **Ruột thanh trạng thái ($512 \times 64$ px | `UI_Bar_Fill_White.png`):**
     *Mẹo tối ưu:* Chỉ cần tạo 1 thanh ruột màu **trắng tinh có vân mực mờ**; khi đưa vào Unity, bạn có thể đổi màu trực tiếp bằng thuộc tính `Color` trên thành phần `Image` thành Đỏ (Khí Huyết), Xanh Ngọc (XP), hoặc Vàng (Linh Lực) mà không cần xuất nhiều file!
     ```text
     horizontal solid pure white bar fill with subtle light grey ink wash texture, flat 2d game UI asset, isolated on black background
     ```
   * **Biểu tượng Lông Vũ Độn Thuật ($128 \times 128$ px | `UI_Icon_DonThuat.png`):**
     ```text
     mystical glowing cyan-white crane feather infused with wind ink trails, sumi-e brush style, isolated on pure white background, 2d game skill icon, centered
     ```

---

### Bước 5.2 – Cắt lưới 9-Slice trong Unity Sprite Editor
Kỹ thuật **9-Slice** giúp các khung Panel, thẻ bài và viền phẩm cấp co giãn thoải mái trên màn hình $1920 \times 1080$ mà họa tiết 4 góc không bao giờ bị kéo méo hay vỡ hạt:

1. Chọn file khung UI trong `Assets/Art/UI_Kit/` $\rightarrow$ tại **Inspector**, đảm bảo **Texture Type** là `Sprite (2D and UI)` và **Mesh Type** là **`Full Rect`**.
2. Bấm nút **Sprite Editor** để mở cửa sổ chỉnh sửa lưới.
3. Nhìn 4 cạnh của hình ảnh sẽ thấy **4 chấm tròn nhỏ màu xanh lá cây** nằm ở giữa mỗi cạnh.
4. Dùng chuột kéo 4 đường viền màu xanh lá đó lùi vào phía bên trong, **nằm vừa khít ngay sau phần hoa văn viền góc** (chia hình thành 9 ô như bàn cờ caro: 4 góc cố định, 4 cạnh co giãn 1 chiều, và ô giữa co giãn tự do).
5. Bấm nút **Apply** ở góc trên bên phải cửa sổ Sprite Editor để lưu.
6. Khi kéo khung này vào UI Canvas trong Unity, ở thành phần **Image**, đổi **Image Type** từ `Simple` sang **`Sliced`** là hoàn tất.

---

## PHẦN V: CHECKLIST KIỂM TRA TRƯỚC KHI CHUYỂN SANG LẬP TRÌNH

- [ ] Toàn bộ 4 Lưu phái nhân vật, 5 Yêu thú và 2 Boss đã tách sạch nền trắng, đặt kích thước chuẩn và chỉnh `Pivot = Bottom`.
- [ ] Toàn bộ 6 Pháp bảo, 6 Đan dược, 6 Đạo quả, 5 Nhãn lưu phái và 4 Vật phẩm rơi đã xuất đúng kích thước ($256 \times 256$ và $128 \times 128$) vào `Assets/Art/Icons/`.
- [ ] Bộ khung UI (`Panel`, `Card`, `5 Rarity Borders`, `Bar Frame`) đã được cắt lưới **9-Slice** chuẩn xác trong Sprite Editor.
- [ ] Không có bất kỳ file ảnh nào bị dính nền trắng đục hoặc sai tên thư mục trong cấu trúc 12 ngăn.