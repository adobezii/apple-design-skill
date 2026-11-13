---
name: apple-web-craftsmanship
description: Apple 級 Web 體驗與工藝規範。構建、改寫或審查高質感前端、Landing Page、品牌文案、Bottom Sheet、抽屜、卡片拖拽、彈簧動效、Dark Mode 深色層級、Liquid Glass 液態玻璃，或監控台、GIS 地圖、資料表介面時使用；用戶提及「好看一點」「順一點」「排版怪」「動畫卡」「像 Apple」時強制載入。先分級：A 品牌頁全套排印、光學字階、物理與認知結構；B 導航與手勢組件強制物理、Liquid Glass 控制層與結構、排印僅限宣傳文案；C 監控台、地圖、資料表等讀數介面禁止盤古之白、NBSP、光學標點、孤行與數據層毛玻璃，僅留按下即反饋、1:1 跟手、Dark Mode 語意色階與 1992 HIG 認知結構規範（感知穩定性、黑白先行、生理避藍、無模態、漸進揭露、動詞按鈕與省略號契約）。阻尼比必須經公式換算後寫入 stiffness/damping；bounce 非阻尼比，duration 非響應時間。與 apple-design 互補。
---

# Apple Web Craftsmanship

提煉自 1992《Macintosh HIG》、WWDC 2018《Designing Fluid Interfaces》、WWDC 2019《Dark Mode》、WWDC 2020《UI Typography》、WWDC 2025《Liquid Glass》與 Apple 官網排印標準。執行前先判定場景分級。

## 1. 場景分級矩陣 [最高優先級]

**裁決準則**：介面核心用途為「讀數或掃描高密數據」時，一律歸為 **C 類**（套用 A 類排印或數據層毛玻璃會破壞欄位對齊與讀數清晰度）。

| 級別 | 涵蓋場景 | 微觀排印與光學字階 | 材質、深色層級與幾何 | 流體物理 | 1992 HIG 認知結構 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **A. 品牌** | Landing、Slogan、產品介紹、活動彈窗 | **全套啟用** | 同心圓角 + Dark Mode Elevation（僅頂部懸浮導航可用 Liquid Glass） | **全套啟用** | 動詞契約與容錯原則 |
| **B. 導航與手勢** | Bottom Sheet、側欄、卡片拖拽、模態窗 | **僅限組件內宣傳文案**；工具列/標籤/按鈕禁 `nowrap` 與標點拆分 | **全套啟用**（Liquid Glass 限懸浮控制層、玻璃禁疊玻璃、`[data-surface="elevated"]`） | **全套啟用** | **全套啟用**（非阻塞模態、漸進揭露、動詞與省略號） |
| **C. 監控與數據** | 遙測台、GIS、指標卡、高密表格 | **嚴禁啟用**（保持等寬與緊湊，禁 NBSP、光學位移、孤行、盤古之白） | Dark Mode 語意色階 + 同心圓角；**數據卡片與表格嚴禁使用 Liquid Glass** | **僅留兩項**：按下即反饋、拖拽 1:1 跟手。**禁用**彈簧、投射、橡皮筋 | **強制全套執行**（感知穩定性、黑白先行、生理避藍、無模態） |

## 2. 1992 HIG 認知與結構鐵律 [B / C 類核心，A 類取第 5 點]

1. **感知穩定性 (Perceived Stability)**：嚴禁因狀態切換隱藏或移除按鈕與欄位導致版面跳動 (Zero CLS)。不可用控制項保留原位並置灰 (`disabled` 或降低不透明度)；動態數值強制等寬數字 `font-variant-numeric: tabular-nums;`。
2. **黑白先行與色彩生理學 (B&W First & Beware of Blue)**：
   - 狀態告警 (正常 / 警告 / 斷線) 必須同時具備色彩與圖標形狀、邊框或字重差異，確保轉灰階後仍可辨識。
   - 視網膜中央凹缺乏藍光視錐細胞，**小字號讀數、細線圖表與微型狀態點嚴禁使用高飽和純藍色**，避免邊緣失焦發虛。
3. **無模態與非阻塞 (Modelessness)**：監控與操作台嚴禁使用全屏黑遮罩鎖死畫面。確認或配置面板必須允許拖動移位 (Movable) 或採用局部 Popover，確保可同時監看背景即時數據。
4. **漸進式揭露 (80% Solution)**：預設視圖僅展示 80% 高頻核心指標 (如 RTT、RSRP、車速)，將 20% 診斷級參數與低頻設定收折於二級抽屜或展開列，防止「功能瀑布」淹沒主畫面。
5. **語意契約 (動詞按鈕與省略號)**：
   - 確認框按鈕強制使用具體動作動詞 (如 `[切斷連線]`、`[套用配置]`)，**嚴禁使用含糊的 `[確定 / OK]` 或 `[是 / 否]`**。
   - 立即生效按鈕不加標點；點擊後需彈窗補充參數或二次確認的按鈕，末尾強制加省略號 `…` (如 `匯出日誌…`)。

## 3. 材質、深色層級與幾何 [WWDC 2019 & 2025]

- **同心圓角公式 (Concentricity)**：外層容器圓角必須等於內層元素圓角加內距，消除夾角擠壓感：
  $$R_{\text{outer}} = R_{\text{inner}} + \text{Padding}$$
- **Dark Mode 抬升層級 (WWDC 2019 Elevation)**：
  - 深色模式下禁用黑色投影區分前後層級；Z 軸越高背景明度越高，並用 `1px` 頂部微弱鏡面高光勾勒邊界 (`inset 0 1px 0 rgba(255,255,255,0.12)`)。
  - **雙上下文自動抬升**：Base 環境 (`#000000` -> `#1C1C1E` -> `#2C2C2E`)；在 Modal / Sheet / Popover 宣告 `data-surface="elevated"` 時，內部自動上移一階 (`#1C1C1E` -> `#2C2C2E` -> `#3A3A3C`)。
  - 深色次級文字採用冷藍灰透明基底 `rgba(235, 235, 245, α)` (`0.60` / `0.30` / `0.16`)；深色強調色須提高明度 (如 Blue `#007AFF` -> `#0A84FF`，Red `#FF3B30` -> `#FF453A`)。
- **Liquid Glass 液態玻璃規範 (WWDC 2025)**：
  - **嚴格層級隔離**：僅限用於**懸浮功能控制層** (Tab Bar、Toolbar、Floating Action Button、Popover)；**嚴禁**用於內容卡片、長文背景及 C 類讀數表格本體。
  - **玻璃禁疊玻璃 (No Glass on Glass)**：Liquid Glass 容器內部子按鈕與選中態必須使用實色半透明填充 (`rgba(120,120,128,0.2)`)，嚴禁嵌套第二層 `backdrop-filter`。
  - **Web 雙層實現**：預設 (Safari / Firefox) 提供高飽和毛玻璃 `blur(20px) saturate(180%)` 與雙層鏡面高光框；Chromium 透過 `@supports (backdrop-filter: url(#test))` 結合 `colorInterpolationFilters="sRGB"` 與動態 SDF 邊緣法線貼圖 `<feDisplacementMap>` 模擬凸透鏡邊緣折射（中心維持中性灰 `128, 128` 零失真）。

## 4. 微觀排印與光學字階 [WWDC 2020 + CJK，僅限 A 類及 B 類宣傳文案]

- **20px 光學尺寸切換 (WWDC 2020)**：
  - `< 20px` 屬 `SF Pro Text` 區間（Tracking 隨字號增大由 `11px` 的 `+0.0055em` 漸變至 `17px` Body 的 `-0.024em`）；`>= 20px` 跨入 `SF Pro Display` 區間（`20px` Title 3 處重置為 `+0.019em`，再隨字號增大收緊至 `64px` Display 的 `+0.003em`）。
  - 非 SF Pro 降級字體在 `>= 20px` 大標題需設 `-0.015em` 至 `-0.025em`；繁體中文段落 (`:lang(zh-Hant)`) 歸零負向 `letter-spacing` 避免筆畫擠壓。
- **混排與字體棧**：
  - 盤古之白：優先用 CSS `text-autospace: normal; text-spacing-trim: space-all;`，不支援時文案手動加半形空格。中文與全形標點間嚴禁空格。
  - 字型棧：`-apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", "PingFang TC", "PingFang HK", "PingFang SC", sans-serif`（繁體介面 `TC` 必須在 `SC` 前）。
  - 大標題標點收窄：可加 `font-feature-settings: "halt" on, "chws" on;`；多數 Web 字體無此特性，必須搭配光學標點校正。
- **防腰斬、孤行與斷行**：
  - 含空格之品牌與型號強制用 `&nbsp;` (如 `iPhone&nbsp;18&nbsp;Pro&nbsp;Max`)；普通敘述句空格嚴禁替換。
  - 段落最後 2-4 字連同句尾標點、行尾成語或動賓短語包覆 `<span className="nowrap">`，禁止整段包覆。
  - 長標題於主謂或轉折處手動插入 `<br className="br-sm-only" />`（Tailwind 用 `sm:hidden`，同一斷點禁混用兩套）。
- **居中句號光學校正**：全形句號墨水偏左下，`text-align: center` 時視覺偏左。將句號移出寬度計算（`top`/`left`/`right`/`bottom` **必須全部留空**，寫 `top: 0` 會導致標點飛至容器頂角）：

```html
<h1 class="headline-optical-center">
  <span class="nowrap">想升級，時機正正好<span class="optical-punct">。</span></span>
</h1>
```

*備選方案（三選一）：整行 `padding-left: 0.5em`，或標點後接 `margin-right: -0.5em` 空節點。*

## 5. 流體物理 [WWDC 2018，A/B 類全套；C 類僅按下反饋與 1:1 跟手]

### 五大物理鐵律
1. **按下即反饋**：走 `:active` 或 `pointerdown`，禁等 `click`，禁加影響手感的 debounce。拖拽表面禁用 `manipulation`，設為 `touch-action: none`（或鎖定單軸）。
2. **隨時可抓回**：動畫中再次按下 (`pointerdown`)，立即對進行中的 `animate` 呼叫 `.stop()`，直接從畫面當前渲染值接手，禁止從邏輯終點跳回。
3. **速度原樣交接**：鬆手速度直接作為彈簧初速傳入（禁歸零、禁重複除以 1000）。
4. **空間對稱**：原路進出；彈層 `transform-origin` 必須落在觸發按鈕上。
5. **動態減免**：於事件觸發時即時讀取 `window.matchMedia('(prefers-reduced-motion: reduce)').matches`（禁止在 render 階段快取），啟用時直接到位或改用短透明度過渡。

### 彈簧參數換算與停點算法
Apple 參數為阻尼比 $\zeta$ 與響應時間 $T$ (秒)。**嚴禁將 $\zeta$ / $T$ 直接填入 Motion 的 `damping` / `stiffness`**（$\zeta = 1$ 填入 `damping` 幾乎無阻尼）。質量取 $m = 1$ 時：$k = (2\pi/T)^2$，$c = 4\pi\zeta/T$。Motion 的 `bounce: 0.2` 不等於 $\zeta = 0.8$，`duration` 亦非 $T$。

| 交互場景 | $\zeta$ | $T$ | stiffness ($k$) | damping ($c$) | $\zeta = 0.8$ 啟用條件 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 位移、分頁、重排 | 1.0 | 0.4s | 247 | 31 | 禁用 |
| 確認框、選單 | 1.0 | 0.35s | 322 | 36 | 禁用 |
| 抽屜、側欄 | 0.8 / 1.0 | 0.3s | 439 | 甩動 34 / 慢放 42 | **僅限甩動鬆手**；慢放維持 $\zeta = 1.0$ |
| 旋轉、翻轉 | 0.8 | 0.4s | 247 | 25 | 僅限帶慣性之翻轉 |

- **二元短行程 (抽屜開/關)**：速度 $\vert{}v\vert{} > 700\text{-}900\text{ px/s}$ 沿速度方向提交，否則依中線判定。**禁止**對短行程套用滾動衰減。
- **多停點長行程**：先以指數衰減投射落點 $Endpoint = Current + (v/1000) \times \frac{d}{1 - d}$（100-200px 間距用 $d = 0.99$，整頁長列表用 $d = 0.998$），吸附最近停點並交接原速度。
- **越界橡皮筋**：$f(x) = \frac{x \cdot dim \cdot 0.55}{dim + 0.55 \cdot \vert{}x\vert{}}$。使用 Motion `drag="y"` 時直接用 `dragElastic` 並設 `dragMomentum={false}`，**禁止**在 `onDrag` 內重複 `y.set`。

## 6. 核心 CSS Tokens 與標準代碼範本

```css
/* apple-craftsmanship.css */
:root {
  color-scheme: light dark;
  --font-apple-sans: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display",
    "PingFang TC", "PingFang HK", "PingFang SC", sans-serif;

  /* WWDC 2019 Elevation & Semantic Colors */
  --bg-primary: #ffffff;
  --bg-secondary: #f2f2f7;
  --bg-tertiary: #ffffff;
  --label-primary: rgba(0, 0, 0, 1);
  --label-secondary: rgba(60, 60, 67, 0.6);
  --label-tertiary: rgba(60, 60, 67, 0.3);
  --separator-translucent: rgba(60, 60, 67, 0.29);
  --tint-blue: #007aff;
  --tint-green: #34c759;
  --tint-red: #ff3b30;
  --tint-orange: #ff9500;
  --elevated-shadow: 0 12px 32px rgba(0, 0, 0, 0.12);

  /* WWDC 2025 Liquid Glass Chrome */
  --glass-bg: rgba(255, 255, 255, 0.48);
  --glass-specular-top: rgba(255, 255, 255, 0.85);
  --glass-specular-bottom: rgba(255, 255, 255, 0.25);

  font-family: var(--font-apple-sans);
  font-optical-sizing: auto;
  text-autospace: normal;
  text-spacing-trim: space-all;
  -webkit-font-smoothing: antialiased;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #000000;
    --bg-secondary: #1c1c1e;
    --bg-tertiary: #2c2c2e;
    --label-primary: rgba(255, 255, 255, 1);
    --label-secondary: rgba(235, 235, 245, 0.6);
    --label-tertiary: rgba(235, 235, 245, 0.3);
    --separator-translucent: rgba(84, 84, 88, 0.6);
    --tint-blue: #0a84ff;
    --tint-green: #30d158;
    --tint-red: #ff453a;
    --tint-orange: #ff9f0a;
    --elevated-shadow: inset 0 1px 0 0 rgba(255, 255, 255, 0.12), 0 16px 40px rgba(0, 0, 0, 0.6);
    --glass-bg: rgba(34, 34, 38, 0.54);
    --glass-specular-top: rgba(255, 255, 255, 0.3);
    --glass-specular-bottom: rgba(255, 255, 255, 0.08);
  }
  [data-surface="elevated"] {
    --bg-primary: #1c1c1e;
    --bg-secondary: #2c2c2e;
    --bg-tertiary: #3a3a3c;
  }
}

/* WWDC 2020 Optical Typography Classes */
.type-body { font-size: 1.0625rem; line-height: 1.375rem; letter-spacing: -0.024em; }
.type-title-3 { font-size: 1.25rem; line-height: 1.5625rem; letter-spacing: 0.019em; }
.type-large-title { font-size: 2.125rem; line-height: 2.5625rem; letter-spacing: 0.011em; font-weight: 700; }
:lang(zh-Hant) .type-body { letter-spacing: 0em; }

/* Utilities & Liquid Glass */
.tabular-nums { font-variant-numeric: tabular-nums; }
.nowrap { white-space: nowrap; }
.headline-optical-center { position: relative; text-align: center; }
.optical-punct { position: absolute; pointer-events: none; }
@media (min-width: 640px) { .br-sm-only { display: none; } }

.pressable { touch-action: manipulation; transition: transform 100ms ease-out; }
.pressable:active { transform: scale(0.97); }

.liquid-glass-chrome {
  background: var(--glass-bg);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  box-shadow: inset 0 1px 0.5px 0 var(--glass-specular-top),
              inset 0 -1px 0.5px 0 var(--glass-specular-bottom),
              0 12px 32px -6px rgba(0, 0, 0, 0.16);
}
@supports (backdrop-filter: url(#test)) {
  .liquid-glass-chrome { backdrop-filter: var(--liquid-filter, blur(20px) saturate(180%)); }
}
```

```typescript
// physics.ts
export function springFromApple({ dampingRatio, response }: { dampingRatio: number; response: number }) {
  return {
    stiffness: (2 * Math.PI / response) ** 2,
    damping: (4 * Math.PI * dampingRatio) / response,
    mass: 1,
  };
}

export function calculateRubberband(offset: number, dimension: number, constant = 0.55) {
  return (offset * dimension * constant) / (dimension + constant * Math.abs(offset));
}

export function projectMomentum(position: number, velocity: number, decelerationRate = 0.998) {
  return position + (velocity / 1000) * decelerationRate / (1 - decelerationRate);
}
```

```tsx
// AppleFluidSheet.tsx (整合 Elevated 上下文、同心圓角 R_outer = 28px = 12px + 16px、可中斷彈簧)
import { useRef } from 'react';
import {
  animate, motion, useMotionValue,
  type AnimationPlaybackControls, type PanInfo,
} from 'framer-motion';
import { springFromApple } from './physics';

const OPEN = 0, CLOSED = 360, FLING = 800;

export function AppleFluidSheet() {
  const y = useMotionValue(OPEN);
  const flight = useRef<AnimationPlaybackControls | null>(null);

  function stopFlight() {
    flight.current?.stop();
  }

  function onDragEnd(_: MouseEvent | TouchEvent | PointerEvent, info: PanInfo) {
    const velocity = info.velocity.y;
    const flung = Math.abs(velocity) > FLING;
    const target = flung
      ? (velocity > 0 ? CLOSED : OPEN)
      : (y.get() > CLOSED / 2 ? CLOSED : OPEN);

    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
      y.set(target);
      return;
    }

    const spring = springFromApple({ dampingRatio: flung ? 0.8 : 1, response: 0.3 });
    flight.current = animate(y, target, { type: 'spring', ...spring, velocity });
  }

  return (
    <motion.div
      data-surface="elevated"
      style={{ y, background: 'var(--bg-primary)', boxShadow: 'var(--elevated-shadow)' }}
      drag="y"
      dragConstraints={{ top: OPEN, bottom: CLOSED }}
      dragElastic={0.2}
      dragMomentum={false}
      onPointerDown={stopFlight}
      onDragEnd={onDragEnd}
      className="fixed inset-x-0 bottom-0 mx-auto max-w-lg touch-none select-none rounded-t-[28px] p-4"
    >
      <div className="mx-auto mb-5 h-1 w-10 rounded-full bg-neutral-400/50" />
      <h2 className="headline-optical-center type-title-3 font-semibold" style={{ color: 'var(--label-primary)' }}>
        <span>升級你的工作流</span>
        <br className="br-sm-only" />
        <span className="nowrap">
          體驗極致效能<span className="optical-punct">。</span>
        </span>
      </h2>
      <p className="mt-2 text-center text-sm leading-relaxed" style={{ color: 'var(--label-secondary)' }}>
        專為&nbsp;<strong>iPad&nbsp;Pro&nbsp;(M4)</strong>&nbsp;調整。
        多軌任務切換時，仍留得住<span className="nowrap">餘裕。</span>
      </p>
      <div className="mt-5 rounded-[12px] p-3 text-center" style={{ background: 'var(--bg-secondary)' }}>
        <button type="button" className="pressable rounded-full px-6 py-2.5 text-sm font-medium text-white" style={{ background: 'var(--tint-blue)' }}>
          立即啟用
        </button>
      </div>
    </motion.div>
  );
}
```

## 7. 輸出前自檢閉環 (Verification Hook)

[生成或審查交付前強制自檢：(1) 場景分級是否明確，C 類讀數介面是否已剔除所有排印包裝、彈簧/投射/橡皮筋與數據層毛玻璃？ (2) B/C 類是否落實 1992 HIG 感知穩定性 (置灰不隱藏、tabular-nums)、黑白先行、避用高飽和純藍細字/細線、無模態與動詞/省略號契約？ (3) 材質與色彩是否遵守同心圓角 ($R_{\text{outer}} = R_{\text{inner}} + \text{Padding}$)、Dark Mode `[data-surface="elevated"]` 抬升、Liquid Glass 僅限控制層且玻璃禁疊玻璃？ (4) 字階是否遵守 `20px` Text/Display 光學分界且繁中段落 (`:lang(zh-Hant)`) 歸零負字距？ (5) 按下反饋是否走 `:active`/`pointerdown` 且中途觸碰有呼叫 `.stop()`？ (6) 彈簧是否經 `springFromApple` 換算、開啟 `dragMomentum={false}` 且事件內即時讀取 `matchMedia`？ (7) A 類品牌型號 `&nbsp;`、段尾 2-4 字 `.nowrap`、窄屏片語斷行、無定位座標之 `.optical-punct` 是否合規？]
