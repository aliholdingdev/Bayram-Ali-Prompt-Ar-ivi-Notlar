# ==============================================================================
# [EL-10] OPENCODE F2 DEBUGGER & F5 EFFORT ENGINE MASTER PLAN
# ==============================================================================
# Hedef Dizin: C:\www\versacoder\opencode
# Hedef Bileşenler: tui, ui, core, codemode
# Mimar: Bayram Ali Senior Engineering Identity
# Format: EL10 (Enterprise Level 10 - Expert Prompt & Implementation Plan)
# ==============================================================================

---
inclusion: always
priority: maximum
execution: strict
architecture: clean-architecture-solid
---

## 1. GÖREV KİMLİĞİ VE SİSTEM BAĞLAMI (CONTEXT & IDENTITY)

Sen, 50 yıllık derin mühendislik tecrübesine sahip, TUI/GUI tasarım sistemleri, Effect-TS, SolidJS ve Agentic orkestrasyonunda uzman **Senior Principal System Architect**'sin.
Senin görevin; mevcut `C:\www\versacoder\opencode` mimarisini bozmadan, ona mükemmel şekilde entegre olan yepyeni iki temel sistem inşa etmektir:
1. **F2 Agentic Agent Debugger Sistemi:** Tüm otonom süreçlerin, dinamik vault yüklemelerinin, skill tespitlerinin ve hata ayıklama verilerinin görselleştiği merkezi izleme (telemetri) paneli.
2. **F5 Özel Modelden Bağımsız Agentic Agent Effort Sistemi:** Model seçimini (F6'ya bırakarak) sistemden ayıran, tamamen Opencode'un içsel görev zorluk/kapsam yönetimini (Low, Medium, High, Ultra vb.) sağlayan otonom karar destek mekanizması.

## 2. F2 AGENTIC DEBUGGER MİMARİSİ (TELEMETRY & OBSERVABILITY)

F2 paneli sıradan bir log ekranı değildir; ajanın "zihnini" ve karar alma sürecini şeffaflaştıran bir **Glassbox (Cam Kutu)** arayüzüdür.

### 2.1. Bileşen İşlevleri (packages/tui/src/component/dialog-debugger.tsx)
- **Vault İzleyici (Vault Monitor):** `.ai/` klasöründen o an hangi dosyaların belleğe alındığını (JIT Load) gösterir (Örn: `.ai/architecture/tui.md (1.2KB)`).
- **Aktif Skill Tablosu (Skill Registry):** O an görev için hangi skillerin auto-load edildiğini (Örn: `[ACTIVE] skill-effect-ts-core`) gösterir.
- **Niyet Çözümleyici (Intent Parser View):** Kullanıcının promptundan çıkarılan ham intentleri ve eşleşen kategorileri listeler.
- **Canlı Ağacı (Execution Tree):** Yürütülen işlemlerin hiyerarşik başarı/hata durumlarını (Success, Retry, Fail) görselleştirir.

## 3. F5 ÖZEL EFFORT SİSTEMİ MİMARİSİ (MODEL BAĞIMSIZ)

F5 artık bir yapay zeka modeli seçme ekranı (OpenAI, Claude vb.) DEĞİLDİR. Sadece görev eforunu ve kapsamını (Effort/Context Limit) belirler. Model varyantları F6 kısayolunda yaşayacaktır.

### 3.1. Effort Seviyeleri (packages/tui/src/component/dialog-effort.tsx)
Sistem **Maksimum 10 Seviyeli** bir yapılandırmaya sahip olmalıdır. Varsayılan seviye "Auto" veya "Plan: Medium" olmalıdır.
1. `Auto` (Niyet analiz motoru tarafından otomatik belirlenir)
2. `Plan: Low` (Doğrudan eylem, minimal açıklama)
3. `Plan: Medium` (Standart mimari planlama)
4. `Plan: High` (Derin analiz, yan etki tespiti)
5. `Code: Fast` (Sadece ilgili dosyada hızlı değişiklik)
6. `Code: Standard` (SOLID ve Clean Code standartlarında tam refactor)
7. `Code: Deep Refactor` (Tüm bağımlılıkları etkileyen, kapsamlı onarım)
8. `Task: Lightweight` (Düşük bellek tüketimi, basit I/O işlemleri)
9. `Task: Extreme` (Çoklu-Ajan (Swarm) gerektiren çok dosyalı işlemler)
10. `UltraCode / Max Reasoning` (Maksimum context boyutu, 5 aşamalı Ultrathink mekanizması devrede)

## 4. UI/TUI ENTEGRASYONU VE BİLEŞENLER (PRESENTATION LAYER)

- **Klavye Kısayolları (Keybinds):** `packages/tui/src/component/prompt/index.tsx` ve global listener'lar güncellenecek.
  - `F2` -> `dialog.open(DialogDebugger)`
  - `F5` -> `dialog.open(DialogEffort)`
  - `F6` -> `dialog.open(DialogModelVariant)` (Eski F5 işlevi buraya kaydırılacak).
- **Görsel Dil (Theming):** TUI temalarına (Vercel, Kanagawa, vs.) tam uyumlu, SolidJS `createMemo` ve `createSignal` kullanılarak reaktif bir UI oluşturulacak. Ekranlarda ASCII sanatları, net border'lar ve renk kodlamaları (Kırmızı: Hata, Yeşil: Aktif, Mavi: Yükleniyor) kullanılacak.

## 5. DURUM (STATE) VE LOCAL CONTEXT YÖNETİMİ (EFFECT-TS & SOLIDJS)

- **Local Store Güncellemesi:** `packages/tui/src/context/local.tsx` içerisinde F5 ve F2 için yeni store state'leri tanımlanacak.
- `local.model.effort.current()` reaktif sinyali, F5 ekranından güncellenecek ve tüm prompt oluşturma süreçlerini (System Prompt Builder) dinleyecek.
- Effect-TS üzerinden asenkron event'ler (`Effect.try`, `Effect.gen`) F2 Debugger'a canlı olarak akıtılacak. (Event Emitter veya RxJS / Solid sinyalleri ile).

## 6. DİNAMİK YÜK TEKNOLOJİSİ (JIT & ZERO-BLOAT)

- F2 Debugger ekranı belleği şişirmemelidir. Loglar FIFO (First In First Out) mantığıyla tutulmalı, sadece son 50-100 işlem hafızada tutularak ekrana basılmalıdır.
- F5 Effort seviyesi, dinamik vault'tan (AI bağlamı) ne kadar veri okunacağını belirler. (Örn: `Effort: Low` ise sadece `index.md` okunur, `Effort: High` ise tüm `architecture/` dizini okunur).

## 7. UYGULAMA ADIMLARI VE FAZLAR (EXECUTION ROADMAP)

1. **Faz 1: State ve Core Hazırlığı:** `local.tsx` ve Effect-TS servislerine `DebuggerState` ve genişletilmiş `EffortState` (10 seviyeli) tipleri eklenecek.
2. **Faz 2: F5 Dialog Refactoring:** Mevcut `dialog-effort.tsx` dosyası yeniden yazılarak "Model Bağımlılığı" tamamen kaldırılacak. 10 seviyeli Effort mimarisi uygulanacak.
3. **Faz 3: F6 Model Dialog Eklenmesi:** Eski model seçici F6'ya (`dialog-model.tsx`) taşınacak ve klavye kısayolu atanacak.
4. **Faz 4: F2 Debugger İnşası:** Yepyeni bir `dialog-debugger.tsx` yazılarak, sistem event'lerini canlı listeleyen TUI penceresi oluşturulacak.
5. **Faz 5: Klavye Bağlamaları:** TUI input/keybind motorunda F2, F5, F6 yönlendirmeleri aktif edilecek.

## 8. HATA YÖNETİMİ VE SELF-HEALING (ERROR BOUNDARIES)

- F2 ekranı açılırken veya log akarken hata oluşursa, ana programı (TUI) çöktürmemelidir. ErrorBoundary bileşenleri kullanılacaktır.
- F5'te geçersiz bir effort seviyesi seçilirse, sistem otomatik olarak "Auto" veya "Plan: Medium" durumuna (Self-Heal) dönecektir.

## 9. TEST VE DOĞRULAMA (QA METRİKLERİ)

- **Birim Testleri:** F5 seviyelerinin doğru prompt token limitlerini ve sistem prompt tag'lerini (`web_research_mandatory` vb.) doğru ayarlayıp ayarlamadığı `bun test` ile doğrulanacak.
- **TUI Görsel Doğrulama:** F2 ekranına logların anlık düşüp düşmediği, F5 penceresinin flat list olarak doğru render edildiği test edilecek.
- Typescript strict kuralları (`no any`, `no var`) kesinlikle ihlal edilmeyecektir. Linter ve formatlayıcı (Oxlint/Prettier) hataları sıfır olmalıdır.

## 10. BAŞLATICILAR (TRIGGERS) VE KABUL KRİTERLERİ

**Nasıl Çalıştırılacak:**
Yapay zeka asistanı (Agent), bu planı okuduktan sonra `C:\www\versacoder\opencode` dizininde kodlama aşamasına geçecektir.

**Kabul Kriterleri (DoD):**
- [ ] `packages/tui/src/component/dialog-effort.tsx` F5 Effort Engine standartlarına uymalıdır.
- [ ] `packages/tui/src/component/dialog-model.tsx` (F6) başarıyla ayrıştırılmış olmalıdır.
- [ ] `packages/tui/src/component/dialog-debugger.tsx` (F2) canlı loglama ve skill gösterimini desteklemelidir.
- [ ] Projenin core yapısı bozulmadan derlenmeli ve `bun start` ile TUI arayüzünde F2/F5/F6 kısayolları tepki vermelidir.

> **AGENT İÇİN EYLEM KOMUTU:** Bu planı okuduğunu onayla, soruların varsa sor; yoksa F5, F6 ve F2 TUI bileşenlerini ve state altyapısını oluşturmaya başla!
