# SKYBREAK ZERO ― 蒼穹のレイド ―

> **1ファイルだけで動く横スクロールシューティング / A side-scrolling shooter in a single file**

- 📱 **横長（16:9）必須** — スマホは縦持ちで回転メッセージを表示
- 🔊 **サウンド On/Off** — ミュージック／効果音を個別に切替（すべてWeb Audio合成・アセット不要）
- 🌍 **8言語対応** — 自動判定＋いつでも切替可能
- 💾 **ローカルセーブ** — localStorage にハイスコア・ステージ進行・設定を自動保存
- 🚫 **ネットワーク通信ゼロ** — 外部CDN・画像・音声ファイル不使用、追跡なし

**目次 / Table of Contents**
[日本語](#lang-ja) · [English](#lang-en) · [简体中文](#lang-zh) · [한국어](#lang-ko) ·
[Español](#lang-es) · [Français](#lang-fr) · [Deutsch](#lang-de) · [Português](#lang-pt)

---

<a name="lang-ja"></a>
## 日本語 (ja)

### 概要
`index.html` 1ファイルだけで動作するネオン調の横スクロールシューティングです。ビルド・依存パッケージ・ネットワーク接続は不要で、ブラウザで開くだけで遊べます。ステージ末尾にボスが待ち受け、撃破すると次のステージへ（無限ループ／難易度上昇）。

### 起動方法
1. `index.html` をダブルクリック（またはブラウザにドラッグ）
2. タイトルのデモ飛行が出たら **タップ / Enter** で離陸

セーブが効かない環境ではローカルサーバー経由を推奨します。

```
python3 -m http.server 8000     # → http://localhost:8000/
```

- Android / iPhone：全画面ボタン（⤢）で没入表示。Android は自動で横固定を試みます
- 縦持ちでは「端末を横にしてプレイしてください」のガイダンスを表示

### 操作方法
| 操作 | PC | スマホ / タブレット |
|---|---|---|
| 移動 | 矢印キー / WASD | 画面 anywhere をドラッグ（指の少し上に機体が追従） |
| ショット | **常時自動発射**（Z・Space でも可） | 自動 |
| ボム | X | 右下の **BOMB** ボタン |
| ポーズ | Esc / P | 右上の **II** ボタン |
| スタート／再挑戦 | Enter / Space | タップ |

### ゲームシステム
- **ウェポンレベル 1〜5**：P を拾うと強化。Lv4 以降は自動誘導ミサイルが追加
- **シールド（最大3）**：S で展開、被弾を1回分吸収
- **ボム（最大5）**：B で補充。弾幕・ビームを一掃し全场の敵へ大ダメージ
- **★アイテム**：得点。近付くと自動吸着
- **コンボ倍率**：撃破で上昇（最大 x9.9）、被弾でリセット
- **エクステンド**：80,000点ごとに残機 +1（最大7）
- **敵種別**：ドローン／ダート（高速突っ込み）／追跡機／砲艦／爆撃艇／中ボス／**ボス**
- **ボスの攻撃**：扇状弾・リング弾・誘導ミサイル・**警告線付きスイープビーム**。HP50%以下でフェーズ2（高速化＋増援）
- **難易度**：Easy / Normal / Hard（敵HP・弾速・発射間隔・残機 5/4/3・ボム 4/3/2・スコア倍率に影響）

### ローカルセーブデータ
キー `skybreak_zero_v1`（localStorage）に自動保存されます。

| フィールド | 内容 |
|---|---|
| `best` / `bestStage` / `unlocked` | ハイスコア／最高到達／解放済みステージ（続きから） |
| `plays` / `kills` | プレイ回数／累計撃破数 |
| `lang` / `diff` | 言語・難易度 |
| `music` / `sfx` | サウンド設定 |

保存タイミング：ステージクリア時、ゲームオーバー時、設定変更時、タブ閉じ際。
削除：**設定 → DELETE SAVE DATA**（確認ダイアログあり）。

### 言語とサウンド
- 対応言語：日本語・English・简体中文·한국어·Español·Français·Deutsch·Português
- 初回はブラウザ言語で自動選択。タイトル／設定のチップで即切替＆保存
- サウンドは効果音15種＋ループBGMをすべてWeb Audio APIでリアルタイム合成
- ブラウザの方針により、**最初のタップ／キー入力のあとに音が鳴り始めます**

### カスタマイズ（`index.html` 内）
| 定数・関数 | 意味 |
|---|---|
| `W = 960, H = 540` | 論理解像度（アスペクト比16:9を維持して拡大） |
| `SAVE_KEY` | セーブのキー名 |
| `DIFF.easy / normal / hard` | `hp` `spd` `rate` `lives` `bombs` `score` |
| `buildStage(n)` | ステージ長（44〜70秒）と出現構成・ボス出現タイミング |
| `ET.boss.hp = 380` | ボスの基礎HP（+(ステージ-1)×95で増加） |
| `playerUpdate()` の `spd = 350` | 機体の移動速度 |
| `G.nextExt += 80000` | エクステンドの間隔 |
| `PAL` / `bgInit()` | ステージ配色・視差背景（星／山稜／雲／ストリーク） |

### トラブルシューティング
| 症状 | 対処 |
|---|---|
| 音が鳴らない | 一度タップ／キー入力（自動再生ポリシー）。設定で MUSIC・SFX が ON か確認 |
| セーブが残らない | シークレットモード解除、または `http://localhost` 経由で開く。ブラウザのサイトデータ削除に注意 |
| 画面が縦のまま | 端末を横にする。PCはウィンドウを横長に／⤢で全画面 |
| 動作が重い | 他タブを閉じる。装飾減量は `boom()` のパーティクル数、`bgInit()` の生成数を下げると効きます |
| 構文エラー（過去版） | `Unexpected token '.'` と `DIFF_PLACEHOLDER is not defined` は現行版で修正済み。上書きしてハードリロード（Ctrl/Cmd+Shift+R） |

### 対応環境
Pointer Events・Web Audio・Canvas に対応した最新 Chrome / Edge / Firefox / Safari、Android Chrome、iOS Safari。**外部依存はゼロ**です。

---

<a name="lang-en"></a>
## English (en)

### Overview
A neon side-scrolling shooter that runs from a single `index.html` — no build step, no dependencies, no network. Each stage ends with a boss; defeat it and you move on forever with rising difficulty.

### Run
1. Double-click `index.html` (or drag it into a browser)
2. Tap / press **Enter** on the title to take off

If saving seems ignored, serve locally: `python3 -m http.server 8000` → `http://localhost:8000/`.
Portrait mode shows a rotate hint; the ⤢ button goes fullscreen (and locks landscape on Android).

### Controls
| Action | Keyboard | Touch |
|---|---|---|
| Move | Arrows / WASD | Drag anywhere (ship floats slightly above your finger) |
| Shot | **Always auto-fire** (Z / Space also work) | Automatic |
| Bomb | X | **BOMB** button, bottom-right |
| Pause | Esc / P | **II** button, top-right |
| Start / Retry | Enter / Space | Tap |

### Systems
- **Weapon level 1–5**: grab `P`; from Lv4 homing missiles join in
- **Shield (max 3)**: `S` absorbs one hit per charge
- **Bomb (max 5)**: `B` — clears bullets and beams, heavy damage to everything
- **★ items**: points, auto-magnetised when close
- **Multiplier**: grows per kill up to x9.9, resets when you are hit
- **Extend**: +1 ship every 80,000 points (cap 7)
- **Enemies**: drone, dart, chaser, gunship, bomber, miniboss, **boss**
- **Boss attacks**: fan spread, bullet ring, homing missiles, telegraphed sweeping beam. Below 50% HP it enters phase 2 (faster + reinforcements)
- **Difficulty**: Easy / Normal / Hard (enemy HP, speed, fire rate, lives 5/4/3, bombs 4/3/2, score factor)

### Local save
Stored under localStorage key `skybreak_zero_v1`: `best`, `bestStage`, `unlocked` (continue stage), `plays`, `kills`, `lang`, `diff`, `music`, `sfx`.
Saved on stage clear, game over, settings change and tab close. Delete via **Settings → DELETE SAVE DATA**.

### Language & sound
8 languages (ja, en, zh, ko, es, fr, de, pt), auto-detected from the browser and switchable at any time. All audio — 15 SFX plus a looping soundtrack — is synthesized live with Web Audio; nothing is downloaded. Sound starts after your first tap/keypress (autoplay policy).

### Tuning (`index.html`)
`W/H` logical resolution · `SAVE_KEY` · `DIFF.*` difficulty knobs · `buildStage(n)` stage length & spawn mix · `ET.boss.hp` boss health · `spd = 350` ship speed · `nextExt += 80000` extend interval · `PAL` per-stage palettes.

### Troubleshooting
- **No sound** → tap once, check MUSIC/SFX toggles
- **Save lost** → leave private mode or open through `http://localhost`
- **Still portrait** → rotate the device; on desktop widen the window or use ⤢
- **Low FPS** → close other tabs; lower particle counts in `boom()` / `bgInit()`
- **Old syntax errors** (`Unexpected token '.'`, `DIFF_PLACEHOLDER is not defined`) → fixed in this version; hard-reload (Ctrl/Cmd+Shift+R)

Supports any recent Chrome, Edge, Firefox, Safari, Android Chrome and iOS Safari. Zero external dependencies.

---

<a name="lang-zh"></a>
## 简体中文 (zh)

### 简介
仅靠一个 `index.html` 即可运行的霓虹风横版射击游戏：无需构建、无依赖、不联网。每关末尾有 Boss，击败后无限推进并逐渐变难。

### 启动
1. 双击 `index.html`（或拖入浏览器）
2. 标题界面 **点击 / Enter** 起飞

若存档无效，请用本地服务：`python3 -m http.server 8000` → `http://localhost:8000/`。
竖屏会显示横屏提示；⤢ 按钮进入全屏（Android 尝试锁定横屏）。

### 操作
| 动作 | 键盘 | 触屏 |
|---|---|---|
| 移动 | 方向键 / WASD | 屏幕任意位置拖动（机体略高于手指） |
| 射击 | **始终自动**（Z / Space 亦可） | 自动 |
| 炸弹 | X | 右下 **BOMB** 按钮 |
| 暂停 | Esc / P | 右上 **II** 按钮 |
| 开始／重试 | Enter / Space | 点击 |

### 系统
- **火力 1–5 级**：拾取 `P`；Lv4 起追加自动诱导导弹
- **护盾（最多3层）**：`S`，每层抵挡一次伤害
- **炸弹（最多5枚）**：`B`，清除弹幕与光束并全场高额伤害
- **★ 道具**：得分，靠近自动吸附
- **倍率**：击毁上升，最高 x9.9，被弹归零
- **加机**：每 80,000 分 +1（上限 7）
- **敌人**：无人机／高速机／追踪机／炮舰／轰炸艇／中 Boss／**Boss**
- **Boss 攻击**：扇形弹、环形弹、诱导导弹、**带预警线的扫射光束**；血量低于 50% 进入二阶段（加速＋增援）
- **难度**：简单 / 普通 / 困难（影响敌机血量、弹速、发射间隔、机数 5/4/3、炸弹 4/3/2、分数系数）

### 本地存档
localStorage 键 `skybreak_zero_v1`：`best`、`bestStage`、`unlocked`（继续关卡）、`plays`、`kills`、`lang`、`diff`、`music`、`sfx`。
过关、游戏结束、修改设置、关闭页面时自动保存；**设置 → 删除存档** 可清除。

### 语言与声音
支持 8 种语言（ja, en, zh, ko, es, fr, de, pt），按浏览器语言自动选择，随时切换并保存。15 种音效与循环 BGM 全部由 Web Audio 实时合成，不下载任何素材。受自动播放策略限制，需首次点击／按键后才有声音。

### 可调参数
`W/H` 逻辑分辨率 · `SAVE_KEY` · `DIFF.*` 难度 · `buildStage(n)` 关卡长度与编成 · `ET.boss.hp` Boss 血量 · `spd = 350` 机体速度 · `nextExt += 80000` 加机间隔 · `PAL` 每关配色。

### 故障排查
- **没有声音** → 先点一下画面，检查 MUSIC/SFX 是否开启
- **存档不保存** → 退出无痕模式，或改用 `http://localhost`
- **仍是竖屏** → 旋转设备；桌面端拉宽窗口或按 ⤢
- **掉帧** → 关闭其他标签页；调小 `boom()` / `bgInit()` 的粒子数量
- **旧版报错**（`Unexpected token '.'`、`DIFF_PLACEHOLDER is not defined`）→ 本版本已修复，请强制刷新（Ctrl/Cmd+Shift+R）

支持新版 Chrome / Edge / Firefox / Safari、Android Chrome、iOS Safari，无任何外部依赖。

---

<a name="lang-ko"></a>
## 한국어 (ko)

### 개요
`index.html` 한 파일로 동작하는 네온 사이드 스크롤 슈터입니다. 빌드·의존성·네트워크 연결이 필요 없고, 각 스테이지 끝에 보스가 등장하며 계속 다음으로 진행됩니다.

### 실행
1. `index.html`을 더블클릭(또는 브라우저에 드래그)
2. 타이틀에서 **탭 / Enter** 로 이륙

저장이 되지 않으면 로컬 서버로 실행하세요: `python3 -m http.server 8000` → `http://localhost:8000/`.
세로 모드에서는 회전 안내가 뜨고, ⤢ 버튼으로 전체화면(Android는 가로 고정 시도)으로 전환됩니다.

### 조작
| 동작 | 키보드 | 터치 |
|---|---|---|
| 이동 | 방향키 / WASD | 화면 아무 곳이나 드래그(손가락 바로 위에 기체) |
| 사격 | **항상 자동**(Z·Space 도 가능) | 자동 |
| 봄 | X | 오른쪽 아래 **BOMB** |
| 일시정지 | Esc / P | 오른쪽 위 **II** |
| 시작／재도전 | Enter / Space | 탭 |

### 시스템
- **웨폰 레벨 1~5**: `P` 획득, Lv4부터 자동 유도 미사일 추가
- **실드(최대 3)**: `S`, 한 층당 피격 1회 방어
- **봄(최대 5)**: `B`, 탄막·빔 제거 및 전체 적에게 큰 피해
- **★ 아이템**: 점수, 가까우면 자동 흡착
- **배율**: 격파 시 상승(최대 x9.9), 피격 시 초기화
- **익스텐드**: 80,000점마다 기체 +1(최대 7)
- **적**: 드론 / 다트 / 추격기 / 포함 / 폭격기 / 중보스 / **보스**
- **보스 공격**: 부채탄, 링탄, 유도 미사일, **경고선付き 스위프 빔**. HP 50% 미만에서 페이즈2(고속화＋증원)
- **난이도**: 이지 / 노멀 / 하드(적 HP·탄속·발사 간격·기체 5/4/3·봄 4/3/2·점수 배율)

### 로컬 세이브
localStorage 키 `skybreak_zero_v1`: `best`, `bestStage`, `unlocked`(이어하기 스테이지), `plays`, `kills`, `lang`, `diff`, `music`, `sfx`.
스테이지 클리어, 게임 오버, 설정 변경, 탭 종료 시 자동 저장. **설정 → DELETE SAVE DATA**에서 삭제.

### 언어와 사운드
8개 언어(ja, en, zh, ko, es, fr, de, pt)를 지원하며 브라우저 언어로 자동 선택됩니다. 15종 효과음과 루프 BGM을 Web Audio로 실시간 합성해 음원 파일을 쓰지 않습니다. 자동 재생 정책으로 첫 탭/키 입력 이후 소리가 재생됩니다.

### 튜닝
`W/H` 논리 해상도 · `SAVE_KEY` · `DIFF.*` · `buildStage(n)` 스테이지 길이·등장 구성 · `ET.boss.hp` 보스 체력 · `spd = 350` 기체 속도 · `nextExt += 80000` 익스텐드 주기 · `PAL` 스테이지 색상.

### 문제 해결
- **소리가 없음** → 화면을 한 번 탭, MUSIC/SFX 설정 확인
- **저장이 안 됨** → 시크릿 모드 해제 또는 `http://localhost`로 접속
- **세로 그대로** → 기기를 회전, PC는 창을 넓히거나 ⤢
- **버벅임** → 다른 탭 종료, `boom()` / `bgInit()`의 파티클 수 감소
- **이전 버전 오류**(`Unexpected token '.'`, `DIFF_PLACEHOLDER is not defined`) → 현재 버전에서 수정 완료. 강제 새로고침(Ctrl/Cmd+Shift+R)

최신 Chrome / Edge / Firefox / Safari, Android Chrome, iOS Safari 지원. 외부 의존성 없음.

---

<a name="lang-es"></a>
## Español (es)

### Resumen
Shooter lateral de estilo neón que funciona con un único `index.html`: sin compilación, sin dependencias y sin red. Cada etapa termina en un jefe y el progreso continúa indefinidamente con dificultad creciente.

### Ejecutar
1. Abre `index.html` con doble clic (o arrástralo al navegador)
2. Pulsa **TOCAR / Enter** en el título para despegar

Si no guarda, sirve la carpeta: `python3 -m http.server 8000` → `http://localhost:8000/`.
En vertical aparece un aviso; el botón ⤢ activa pantalla completa (Android intenta fijar horizontal).

### Controles
| Acción | Teclado | Táctil |
|---|---|---|
| Mover | Flechas / WASD | Arrastra en cualquier zona (la nave flota sobre el dedo) |
| Disparo | **Automático siempre** (Z o Espacio también) | Automático |
| Bomba | X | Botón **BOMB**, abajo a la derecha |
| Pausa | Esc / P | Botón **II**, arriba a la derecha |
| Iniciar / Reintentar | Enter / Space | Toque |

### Sistemas
- **Nivel de arma 1–5** con `P`; desde Lv4 misiles teledirigidos
- **Escudo (máx. 3)** con `S`: absorbe un impacto por carga
- **Bomba (máx. 5)** con `B`: limpia balas y rayos, gran daño global
- **★**: puntos, atracción automática al acercarse
- **Multiplicador**: sube al destruir hasta x9.9, se reinicia al recibir un impacto
- **Vida extra**: +1 nave cada 80.000 puntos (máx. 7)
- **Enemigos**: dron, dardo, perseguidor, cañonero, bombardero, subjefe, **jefe**
- **Ataques del jefe**: abanico, anillo, misiles y **haz barrido con aviso previo**. Bajo 50% de vida pasa a fase 2 (más rápido y con refuerzos)
- **Dificultad**: Fácil / Normal / Difícil (vida enemiga, velocidad, cadencia, naves 5/4/3, bombas 4/3/2, factor de puntos)

### Guardado local
Clave `skybreak_zero_v1` en localStorage: `best`, `bestStage`, `unlocked` (continuar), `plays`, `kills`, `lang`, `diff`, `music`, `sfx`.
Se guarda al superar etapas, al perder, al cambiar ajustes y al cerrar. Se borra en **AJUSTES → BORRAR DATOS**.

### Idioma y sonido
8 idiomas (ja, en, zh, ko, es, fr, de, pt) detectados automáticamente y con cambio inmediato. Los 15 efectos y la música en bucle se sintetizan con Web Audio: no se descarga ningún archivo. El audio arrasa tras el primer toque o tecla (política de autoplay).

### Ajustes
`W/H` resolución lógica · `SAVE_KEY` · `DIFF.*` · `buildStage(n)` duración y aparición · `ET.boss.hp` vida del jefe · `spd = 350` velocidad · `nextExt += 80000` ritmo extra · `PAL` paletas.

### Solución de problemas
- **Sin sonido** → toca una vez y revisa MUSIC/SFX
- **No guarda** → sal del modo incógnito o usa `http://localhost`
- **Sigue vertical** → gira el dispositivo; en PC ensancha la ventana o pulsa ⤢
- **Bajos FPS** → cierra pestañas; reduce partículas en `boom()` / `bgInit()`
- **Errores de versiones anteriores** (`Unexpected token '.'`, `DIFF_PLACEHOLDER is not defined`) → corregidos; recarga forzada (Ctrl/Cmd+Shift+R)

Compatible con Chrome, Edge, Firefox, Safari, Android Chrome e iOS Safari. Sin dependencias externas.

---

<a name="lang-fr"></a>
## Français (fr)

### Principe
Shooter horizontal néon tenant dans un seul `index.html` : aucune compilation, aucune dépendance, aucun réseau. Chaque niveau se termine par un boss et la progression se poursuit avec une difficulté croissante.

### Lancement
1. Ouvrez `index.html` par double-clic (glissez-le dans le navigateur)
2. Sur le titre, **touche / Entrée** pour décoller

Si la sauvegarde n'est pas prise en compte : `python3 -m http.server 8000` → `http://localhost:8000/`.
En mode portrait un message s'affiche ; le bouton ⤢ passe en plein écran (verrouillage paysage tenté sur Android).

### Commandes
| Action | Clavier | Tactile |
|---|---|---|
| Déplacement | Flèches / WASD | Glissez n'importe où (le vaisseau flotte au-dessus du doigt) |
| Tir | **Automatique en continu** (Z ou Espace aussi) | Automatique |
| Bombe | X | Bouton **BOMB**, en bas à droite |
| Pause | Esc / P | Bouton **II**, en haut à droite |
| Démarrer / Rejouer | Entrée / Espace | Toucher |

### Systèmes
- **Arme niveau 1 à 5** via `P` ; missiles guidés dès le niveau 4
- **Bouclier (max 3)** via `S` : absorbe un tir par charge
- **Bombe (max 5)** via `B` : efface tirs et rayons, gros dégâts globaux
- **★** : points, attraction automatique à proximité
- **Multiplicateur** : monte jusqu'à x9.9, remis à zéro en cas d'impact
- **Vie bonus** : +1 vaisseau tous les 80 000 points (max 7)
- **Ennemis** : drone, dard, poursuivant, canonnière, bombardier, mini-boss, **boss**
- **Attaques du boss** : éventail, anneau, missiles guidés et **rayon balayé avec signal d'alerte**. Sous 50 % de PV : phase 2 (plus rapide + renforts)
- **Difficulté** : Facile / Normal / Difficile (PV ennemis, vitesse, cadence, vaisseaux 5/4/3, bombes 4/3/2, coefficient de score)

### Sauvegarde locale
Clé localStorage `skybreak_zero_v1` : `best`, `bestStage`, `unlocked` (reprise), `plays`, `kills`, `lang`, `diff`, `music`, `sfx`.
Écrite à la fin d'un niveau, en game over, au changement de réglage et à la fermeture. Effacement via **RÉGLAGES → EFFACER SAUVEGARDE**.

### Langue et son
8 langues (ja, en, zh, ko, es, fr, de, pt) détectées automatiquement et commutables à tout moment. Les 15 effets et la musique en boucle sont synthétisés en Web Audio : aucun fichier téléchargé. Le son démarre après le premier toucher ou appui (politique autoplay).

### Réglages
`W/H` résolution logique · `SAVE_KEY` · `DIFF.*` · `buildStage(n)` durée et vagues · `ET.boss.hp` PV du boss · `spd = 350` vitesse · `nextExt += 80000` palier bonus · `PAL` palettes.

### Dépannage
- **Pas de son** → touchez l'écran, vérifiez MUSIQUE/EFFETS
- **Sauvegarde perdue** → quittez la navigation privée ou passez par `http://localhost`
- **Encore en portrait** → pivotez l'appareil ; sur PC élargissez la fenêtre ou ⤢
- **Chutes de images/s** → fermez des onglets, réduisez les particules dans `boom()` / `bgInit()`
- **Erreurs des versions précédentes** (`Unexpected token '.'`, `DIFF_PLACEHOLDER is not defined`) → corrigées ; rechargement forcé (Ctrl/Cmd+Shift+R)

Compatible Chrome, Edge, Firefox, Safari, Android Chrome et iOS Safari. Zéro dépendance externe.

---

<a name="lang-de"></a>
## Deutsch (de)

### Überblick
Ein Neon-Sidescroller-Shooter in genau einer `index.html`: ohne Build, ohne Abhängigkeiten, ohne Netzwerk. Jede Stufe endet mit einem Boss, danach geht es endlos weiter – mit steigender Schwierigkeit.

### Starten
1. `index.html` per Doppelklick öffnen (oder in den Browser ziehen)
2. Im Titel **Tippen / Enter** zum Start

Fortschritt wird nicht gesichert? Lokal ausliefern: `python3 -m http.server 8000` → `http://localhost:8000/`.
Im Hochformat erscheint ein Hinweis; ⤢ startet den Vollbildmodus (Android versucht die Querformat-Fixierung).

### Steuerung
| Aktion | Tastatur | Touch |
|---|---|---|
| Bewegen | Pfeile / WASD | Beliebig ziehen (das Schiff schwebt über dem Finger) |
| Schuss | **Immer Auto-Feuer** (auch Z / Leertaste) | Automatisch |
| Bombe | X | Button **BOMB**, unten rechts |
| Pause | Esc / P | Button **II**, oben rechts |
| Start / Retry | Enter / Space | Tippen |

### Systeme
- **Waffenlevel 1–5** über `P`; ab Level 4 zielsuchende Raketen
- **Schild (max. 3)** über `S`: pro Ladung ein Treffer absorbiert
- **Bombe (max. 5)** über `B`: löscht Projektile und Strahlen, starker Flächenschaden
- **★**: Punkte, automatische Anziehung in Nähe
- **Multiplikator**: steigt bis x9.9, bei Treffer zurück auf 1
- **Extra-Leben**: +1 Schiff alle 80.000 Punkte (max. 7)
- **Gegner**: Drohne, Dart, Verfolger, Kanonier, Bomber, Mini-Boss, **Boss**
- **Boss-Angriffe**: Fächer, Ring, Raketen und **wischender Strahl mit Warnlinie**. Unter 50 % Lebenspunkte Phase 2 (schneller + Verstärkung)
- **Schwierigkeit**: Leicht / Normal / Schwer (Gegner-HP, Tempo, Feuerrate, Schiffe 5/4/3, Bomben 4/3/2, Punktefaktor)

### Lokaler Speicher
localStorage-Schlüssel `skybreak_zero_v1`: `best`, `bestStage`, `unlocked` (Fortsetzung), `plays`, `kills`, `lang`, `diff`, `music`, `sfx`.
Gespeichert nach Stufe, bei Game Over, bei Einstellungen und beim Schließen. Löschen über **OPTIONEN → SPEICHER LÖSCHEN**.

### Sprache & Sound
8 Sprachen (ja, en, zh, ko, es, fr, de, pt), automatisch erkannt und jederzeit umschaltbar. Alle 15 Effekte und die Loop-Musik werden per Web Audio synthetisiert – keine Downloads. Ton startet nach der ersten Eingabe (Autoplay-Richtlinie).

### Fein tuning
`W/H` logische Auflösung · `SAVE_KEY` · `DIFF.*` · `buildStage(n)` Stufenlänge & Wellen · `ET.boss.hp` Boss-HP · `spd = 350` Tempo · `nextExt += 80000` Extra-Intervall · `PAL` Farbpaletten.

### Fehlerbehebung
- **Kein Ton** → einmal tippen, MUSIK/EFFEKTE prüfen
- **Speicher fehlt** → Inkognito verlassen oder `http://localhost` nutzen
- **Weiter Hochformat** → Gerät drehen; am PC Fenster breiter oder ⤢
- **Wenig FPS** → Tabs schließen, Partikel in `boom()` / `bgInit()` reduzieren
- **Frühere Syntaxfehler** (`Unexpected token '.'`, `DIFF_PLACEHOLDER is not defined`) → behoben; Hard-Reload (Strg/Cmd+Shift+R)

Unterstützt aktuelle Versionen von Chrome, Edge, Firefox, Safari, Android Chrome und iOS Safari. Ganz ohne externe Abhängigkeiten.

---

<a name="lang-pt"></a>
## Português (pt)

### Visão geral
Um shooter lateral neon que roda em um único `index.html`: sem build, sem dependências e sem rede. Cada fase termina com um chefe e o progresso segue indefinidamente com dificuldade crescente.

### Executar
1. Abra `index.html` com dois cliques (ou arraste para o navegador)
2. No título, **toque / Enter** para decolar

Se nada for salvo, sirva localmente: `python3 -m http.server 8000` → `http://localhost:8000/`.
Na vertical aparece um aviso; o botão ⤢ ativa tela cheia (Android tenta fixar a horizontal).

### Controles
| Ação | Teclado | Toque |
|---|---|---|
| Mover | Setas / WASD | Arraste em qualquer área (a nave fica acima do dedo) |
| Tiro | **Automático sempre** (Z ou Espaço também) | Automático |
| Bomba | X | Botão **BOMB**, canto inferior direito |
| Pausa | Esc / P | Botão **II**, canto superior direito |
| Iniciar / Repetir | Enter / Space | Toque |

### Sistemas
- **Arma nível 1–5** com `P`; a partir do nível 4, mísseis teleguiados
- **Escudo (máx. 3)** com `S`: absorve um impacto por carga
- **Bomba (máx. 5)** com `B`: limpa tiros e feixes, grande dano global
- **★**: pontos, atração automática ao aproximar
- **Multiplicador**: sobe até x9.9 e zera ao ser atingido
- **Vida extra**: +1 nave a cada 80.000 pontos (máx. 7)
- **Inimigos**: drone, dardo, perseguidor, canhoneira, bombardeiro, subchefe, **chefe**
- **Ataques do chefe**: leque, anel, mísseis e **feixe varrido com aviso prévio**. Abaixo de 50% de vida entra na fase 2 (mais rápido + reforços)
- **Dificuldade**: Fácil / Normal / Difícil (vida inimiga, velocidade, cadência, naves 5/4/3, bombas 4/3/2, fator de pontos)

### Salvamento local
Chave `skybreak_zero_v1` no localStorage: `best`, `bestStage`, `unlocked` (continuar), `plays`, `kills`, `lang`, `diff`, `music`, `sfx`.
Salvo ao concluir fase, no fim de jogo, ao mudar ajustes e ao fechar. Apague em **AJUSTES → APAGAR DADOS**.

### Idioma e som
8 idiomas (ja, en, zh, ko, es, fr, de, pt) detectados automaticamente e trocáveis a qualquer momento. Os 15 efeitos e a música em loop são sintetizados com Web Audio — nada é baixado. O som começa após o primeiro toque/tecla (política de autoplay).

### Ajustes
`W/H` resolução lógica · `SAVE_KEY` · `DIFF.*` · `buildStage(n)` duração e ondas · `ET.boss.hp` vida do chefe · `spd = 350` velocidade · `nextExt += 80000` intervalo extra · `PAL` paletas.

### Solução de problemas
- **Sem som** → toque na tela e confira MÚSICA/EFEITOS
- **Progresso não salvo** → saia do modo anônimo ou use `http://localhost`
- **Ainda na vertical** → gire o aparelho; no PC alargue a janela ou use ⤢
- **Queda de FPS** → feche abas; reduza partículas em `boom()` / `bgInit()`
- **Erros de versões antigas** (`Unexpected token '.'`, `DIFF_PLACEHOLDER is not defined`) → corrigidos; recarregue com força (Ctrl/Cmd+Shift+R)

Compatível com Chrome, Edge, Firefox, Safari, Android Chrome e iOS Safari. Sem dependências externas.

---

## Estrutura do arquivo / ファイル構成 / File structure

| Bloco | Conteúdo |
|---|---|
| `<style>` | Layout 16:9 letterbox, HUD, telas, botões touch, proteção de orientação retrato |
| HTML | `#stage` → `canvas` + HUD + pads + banner + 5 telas (título, ajustes, como jogar, pausa, resultado) |
| JS | `I18N` / `save` / `A`(audio) / `MUS` / `G`,`P`(estado) / `bg` / `enemies` / `flow` / `render` / input / loop |

## Privacidade & Licença
- Nenhuma requisição de rede, nenhum cookie, nenhum analytics — todos os dados ficam no dispositivo.
- Use, modifique e distribua livremente (sem garantias). / 自由に改変・利用可（無保証）。
````