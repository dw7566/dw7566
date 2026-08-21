# 이재혁 (Jaehyeok Lee)

> 안녕하세요! **AI로 빠르게 만들되, 맞는지는 도메인으로 가려내는** 반도체 공정·계측 데이터 엔지니어입니다.
> 계측 데이터를 믿을 수 있는 판정으로 바꾸고, 그 판정을 보드 위에서 돌립니다.

# 🌟 About Me

🏫  한양대학교 ERICA 차세대반도체융합공학부 반도체디스플레이전공

🕍  **팹리스점프업 2기**

🏆  **2026 ERICA人 AI 학습 활용 사례 공모전 최우수상** (한양대학교 ERICA IC-PBL교수학습센터, 2026.07)

📧  **Email** | dw7566@hanyang.ac.kr

# 🤖 AI를 쓰는 방식

> **AI를 활용한다는 것은 답을 시키는 일이 아니라, 기본 도메인을 정확히 깔아 주어 그 위에서 일하게 하는 일이다.**
>
> — 「정답을 받는 대신, 도메인을 먼저 깔았다」 · 2026 ERICA人 AI 학습 활용 사례 공모전 **최우수상** 에세이

실리콘 포토닉스 웨이퍼 측정 데이터를 다루는 IC-PBL 프로젝트가 출발점이었습니다.
**709개 XML**(4 웨이퍼 × 14 다이 × 13 사이트, 484.8 MB)을 손으로 여는 건 불가능했고,
"생성형 AI로 만들어 보라"는 조건이 붙어 있었습니다.

### 운전대는 내가 잡습니다

| 내가 하는 것 | AI에 맡기는 것 |
|---|---|
| 문제 정의 · 도메인 설명 · 설계 판단 · **결과 검증** | 반복 구현 · 리팩터링 · 테스트 보조 |

### 왜 이렇게 하는가 — 실제로 겪은 실패

- **"이 데이터로 그래프를 그려 줘"** → 모양은 그럴듯한 곡선이 나왔지만 물리적 의미가 실제 측정과 동떨어져 있었습니다. 두 빛의 간섭으로 생기는 투과 곡선의 식과 축의 물리량을 **먼저 깔아 주자** 비로소 분석에 쓸 수 있는 결과가 나왔습니다.
- 배경 제거 보정에서 AI가 제안한 방식이 **분석해야 할 핵심 특징까지 함께 지웠습니다.**
- 곡선 피팅 초기값을 고정했다가 설명력(R²)이 **0에 가깝게 무너졌습니다.** 데이터에서 자동으로 찾게 바꾸자 **0.95**까지 올라갔습니다.

> **AI가 준 답을 그대로 믿었다면 결과는 조용히 틀려 있었을 것입니다.**
> 틀렸다고 말할 수 있었던 건 제가 광소자의 물리를 알았기 때문입니다.

### 그래서 저장소마다 '검증'을 남깁니다

| 원칙 | 저장소에 남은 증거 |
|---|---|
| 출력은 반드시 검증한다 | **음성 대조군** — 변하지 않아야 할 압력이 실제로 sd=0.0000 (`plasma-etch`)<br/>**정답 주입 시험** — 오탐 0건·미탐 0건 (`WaferSense`) |
| 결과물을 남긴다 | 테스트 115건(`WaferSense`) · 47건(`picqa`) · 27건(`plasma-etch`) · 17건(`Apache6`) |
| 핵심 설계는 내가 쥔다 | NPU 제약 안에서 동작하는 LKA 블록 설계 · leverage 기반 불확실성 판정기 |
| 숫자를 부풀리지 않는다 | 미검출 0건을 "0%"가 아니라 **"95% 신뢰상한 5.4%"** 로 표기 |

**AI의 가치는 AI의 성능이 아니라, 그것을 쓰는 사람의 도메인 이해와 검증 능력에 비례합니다.**

---

# 🛠️ 반도체 공정·계측 데이터

> 💡 저는 **공정·계측 데이터에서 판정 기준을 만드는** 데이터 엔지니어 이재혁입니다!

- **공정을 직접 돌려본 데이터 엔지니어**입니다. Oxide TFT를 기판 세정부터 S/D 전극 패터닝까지 전 공정 직접 제작했고, 그 경험 위에서 공정 데이터를 해석합니다.
- **가상계측(Virtual Metrology)** 시스템을 설계한 경험이 있습니다. 센서 3개로 공정 시작 4.8초 시점에 식각 깊이를 예측하고, **불확실성을 함께 산출해 실계측 76.1%를 생략**하면서 규격이탈 미검출 0/67을 달성했습니다.
- **계측 데이터의 신뢰성 판별**을 먼저 설계합니다. 값을 읽기 전에 *어떤 측정을 믿을 수 있는가*를 가려야 한다고 보고, `PASS`/`SUSPECT`/`DEAD` 3등급 판별 계층을 구현했습니다.
- **Cpk·NU·CV 기반 공정능력 분석**으로 "규격 안에 있지만 산포가 커지는" 열화 신호를 잡아냅니다.
- **드리프트의 원인까지 규명**합니다. 상관을 찾는 데서 멈추지 않고 **음성 대조군**(변하지 않아야 할 신호가 실제로 변하지 않았음)을 설계해 논증을 닫습니다.
- 분석에 그치지 않고 **ARM64 보드에 직접 실장**해 PC 대비 오차 1.0×10⁻⁵ µm를 검증했습니다.
- 모든 비율 지표에 **신뢰구간을 병기**합니다. "미검출 0건"은 0%가 아니라 95% 상한 5.4%라고 씁니다.

---

# ✏️ 기술 STACK

## 반도체·디스플레이 공정 도메인

- **Oxide TFT 소자 제작** — 기판 세정 · Mo Gate 패터닝 · Al₂O₃ ALD · ITZO 채널 · S/D 전극까지 전 공정 직접 수행.
- **포토리소그래피** 마스크 얼라인, 박막 증착, 클린룸 환경을 실습으로 경험했습니다.
- **ICP-RIE 플라즈마 식각** — 챔버 상태 드리프트, RF 정합, 폴리머 누적과 식각률의 관계를 데이터로 다뤘습니다.
- **실리콘 포토닉스** — MZ 변조기 · PN 변조기 · 광검출기 · 도파로 소자별 특성 파라미터를 이해하고 추출합니다.
- **검·계측** — SEM 결함 검사, OES 발광분광, Ellipsometry 기반 박막 두께 분석, SAICAS 박막 특성 분석.

## 공정·계측 데이터 분석

- **Python** (NumPy · pandas · SciPy · scikit-learn · netCDF4)으로 대용량 계측 데이터 파이프라인을 구축합니다.
- **실리콘 포토닉스** 웨이퍼 데이터에서 MZM 파라미터(FSR, |dλ/dV|, Peak IL, ER, Vπ·L)와 PN 변조기의 길이 의존성을 추출하는 라이브러리를 설계했습니다.
- **OES 발광분광** 3,648채널을 다뤄 특정 파장 대역의 거동을 lot 간 재현성으로 검증한 경험이 있습니다.
- **XML/netCDF 계측 포맷 파싱**, 레이아웃 비의존 자동 탐색, 리포트 자동 생성(Excel)에 익숙합니다.

## 통계적 판정 설계

- **Leave-One-Lot-Out 교차검증** — 같은 lot이 챔버 상태를 공유하므로 무작위 분할이 성능을 부풀린다는 점을 이해하고 검증 설계를 분리합니다.
- **불확실성 정량화** — Gaussian Process, leverage 기반 예측 분산으로 *얼마나 확신하는가*를 함께 산출합니다.
- **Wilson 신뢰구간**으로 소표본(n=88) 비율 추정의 불확실성을 명시합니다.
- **모델 전수 비교** — 5 특징셋 × 13 모델 = 65조합을 전부 측정해 선택 근거를 남깁니다.
- 표본 크기에 맞는 모델을 고릅니다. n=88에 신경망을 쓰지 않은 이유를 **측정으로** 답할 수 있습니다.

## 임베디드 실장

- **C (C99)** 로 Python 모델을 이식하고 스트리밍 상태머신·예측·불확실성을 구현했습니다.
- **System V IPC**(공유메모리·세마포어)로 다중 챔버 동시 감시 구조를 설계하고 경합을 측정했습니다.
- **ARM64 크로스컴파일**(Cortex-A65AE), 타깃별 최적화 플래그 비교 경험이 있습니다.
- **수치 안정성** — float32 파괴적 상쇄를 발견하고 표준화 형태 + double 누적으로 오차를 0.0486 µm → 9.9×10⁻⁶ µm로 줄인 경험이 있습니다.
- **TensorFlow Lite GPU 델리게이트**로 Mali GPU 추론 파이프라인을 구성하고 CPU 폴백을 구현했습니다.
- **aiWare NPU** 워크플로 전체를 다룹니다 — YOLO 학습 → ONNX Export → **INT8 PTQ 양자화** → NPU 컴파일(aiwbin) → 실보드 mAP 평가.
- **추론 런타임 최적화** — 모델 상주(serve) 구조로 프레임당 419 ms → 20.1 ms(20.9배), MJPEG 프리뷰로 전송량 1/31 절감.
- **NPU 제약을 고려한 모델 설계** — dilation·window 한계 안에서 동작하는 커스텀 어텐션 블록(LKA)을 Ultralytics에 이식했습니다.
- **V4L2 / MIPI CSI 카메라 캡처**, **Wayland / OpenGL ES** 표시 경로를 다뤘습니다.
- **FreeRTOS · CAN** 기반 Zone 컨트롤러와 Qt/QML 클러스터를 연동한 경험이 있습니다.

## 엔지니어링 습관

- **테스트를 남깁니다** — 외부 데이터 없이 도는 검증을 붙여 저장소만 clone하면 핵심 주장이 재현되게 만듭니다.
- **재현 절차와 함정을 문서화합니다** — 실제로 저질렀던 실수(사전 복호 누락으로 R² 0.2 손실 등)를 그대로 적습니다.
- **한계를 먼저 씁니다** — 결론 내리지 못한 것은 내리지 못했다고 남깁니다.

---

# 🏅 교육·실습 이력

## 🏆 2026 ERICA人 AI 학습 활용 사례 공모전 — **최우수상**

`2026.07` · 한양대학교 ERICA IC-PBL교수학습센터 (IC-PBL 2026-033호)

에세이 「**정답을 받는 대신, 도메인을 먼저 깔았다**」로 최우수상을 수상했습니다.
공업프로그래밍 2 IC-PBL 프로젝트에서 실리콘 포토닉스 웨이퍼 데이터 분석 도구를
만들며 정립한 원칙을 정리한 글입니다 → [🤖 AI를 쓰는 방식](#-ai를-쓰는-방식)

그 프로젝트의 결과물이 아래 두 저장소입니다. 처음엔 클릭으로 다루는 GUI로 만들었다가,
교수님 피드백을 거쳐 **누구나 코드로 불러 쓰고 검증할 수 있는 모듈형 라이브러리로 재설계**했습니다.

- **[xml_analyzer_project](https://github.com/dw7566/xml_analyzer_project)** — 초기 GUI 버전
- **[picqa](https://github.com/dw7566/picqa)** — 재설계한 모듈형 라이브러리 + CLI (테스트 47건, CI)

## 🔬 디스플레이 부트캠프 — 박막트랜지스터 공정 실습

`2026.06.24 ~ 06.30` · 한양대학교 ERICA

**Oxide TFT (Bottom Gate 구조)** 를 직접 제작하며 반도체 8대 공정을 실습했습니다.

| 단계 | 공정 | 재료 |
|---|---|---|
| ① | 기판 준비 (세정) | Si / SiO₂ |
| ② | Gate 형성 (패터닝) | Mo |
| ③ | Insulator 증착 (**ALD**) | Al₂O₃ |
| ④ | Channel 증착 및 패터닝 | **ITZO** |
| ⑤ | Source/Drain 형성 (패터닝) | Mo |

- **포토리소그래피** — 마스크 얼라인을 통해 미세 구조를 형성하는 과정을 직접 수행
- Gate 전압 → ITZO 계면 전자농도 증가 → 채널 저항 감소로 이어지는 **TFT 동작 원리**를 소자 제작으로 체득

## 🏭 디스플레이 부트캠프 — OLED 공정 및 XR 현장 실습

`2026.08.06` · 충남테크노파크 혁신공정센터

- OLED 디스플레이 **제조공정 및 팹·클린룸** 견학
- **XR/VR 실습**으로 CVD 장비 구조와 박막 형성 공정을 가상 환경에서 체험
- **검·계측 장비** 견학 — **SAICAS** 박막 스크래치 특성 분석 사례를 통해 측정 결과가 공정 평가로 이어지는 흐름 이해

## 💼 2026-1학기 현직자 직무부트캠프 (G7/GX)

`2026.05.11 ~ 05.29` · ㈜월드클래스에듀케이션 · 산업·직무 교육 과정 이수

---

# 📑 Project

- **[Plasma Etch Virtual Metrology >> https://github.com/dw7566/plasma-etch-virtual-metrology](https://github.com/dw7566/plasma-etch-virtual-metrology)** <br/>
| `Python` `C` `scikit-learn` `ARM64` `System V IPC` `Virtual Metrology` `OES` <br/>
  <a href="https://github.com/dw7566/plasma-etch-virtual-metrology">
    <img width="512" height="auto" alt="etch depth drift" src="https://raw.githubusercontent.com/dw7566/plasma-etch-virtual-metrology/main/figs/fig1_drift.png" /> <br/>
  </a>
  - BOSCH 식각 공정 드리프트를 **인과까지 규명**하고 가상계측 판정기를 임베디드 실장
  - 드리프트 −0.119 µm/wafer (10/10 lot), 원인은 RF 정합 특성 변화 — 압력은 88장 전체 sd = 0.0000 (음성 대조군)
  - LOLO R² 0.855 · **실계측 76.1% 생략, 규격이탈 미검출 0/67** (95% 상한 5.4%)
  - C 586줄 ARM64 이식, PC 대비 오차 **1.0×10⁻⁵ µm**, 누적 샘플 88/88 일치
  - OES 3,648채널에서 543–562 nm 대역 감소를 **4개 lot 전부 재현** (p<0.02)

- **[WaferSense >> https://github.com/dw7566/wafersense](https://github.com/dw7566/wafersense)** <br/>
| `Python` `Cpk` `Streamlit` `Tkinter` `LLM` `pytest` `PyInstaller` <br/>

  ### ▶ 시연 영상 (71초) — 이미지를 클릭하면 재생됩니다
  <a href="https://github.com/dw7566/dw7566/blob/main/assets/wafersense/wafersense_demo.mp4">
    <img width="640" height="auto" alt="WaferSense 판별 결과" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/wafersense/verdict_summary.png" /> <br/>
  </a>

  > **계측 데이터 분석의 첫 번째 병목은 값을 읽는 일이 아니라, 어떤 측정을 믿을 수 있는지 가려내는 일이다.**

  프로브 접촉 불량이나 장비 오작동으로 생긴 **무효 측정이 통계에 섞이면 공정 판단 자체가 오염**됩니다.
  WaferSense는 계측 XML을 넣으면 무효 측정을 자동 격리하고, 규격 대비 산포로 공정능력을 평가한 뒤,
  자연어 리포트까지 생성합니다.

  - **3등급 신뢰성 판별** — 모든 측정에 `PASS` / `SUSPECT` / `DEAD` 등급과 **사유**를 기록합니다. 데이터를 삭제하지 않으므로 **필터링 자체가 감사 가능**합니다.
  - **공정능력 지표** — 웨이퍼 × 밴드별 NU · CV · **Cpk** 를 산출해, 규격 안에 있지만 산포가 확대되는 **열화 신호**를 포착합니다. 가장 취약한 항목을 자동으로 지목합니다.
  - **웨이퍼맵** — 다이 위치별 분포를 지표별로 확인하고, 무효 판정 사유를 세션 단위로 집계합니다.
  - **다중 LLM 리포트** — Anthropic · Gemini · OpenAI · OpenRouter 키를 접두사로 자동 인식하고, 모델이 사용 중단(404)되면 다음 세대로 자동 재시도합니다.
  - **세 가지 실행 방식** — CLI · 데스크톱 GUI(Tkinter) · 웹 GUI(Streamlit) + Windows `.exe` 빌드.

  | 웨이퍼맵 — 다이 위치별 분포 | **검증 — 정답을 아는 데이터로 시험** |
  |---|---|
  | <img alt="wafermap" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/wafersense/wafermap.png" /> | <img alt="validation" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/wafersense/validation.png" /> |

  **검증을 설계했습니다.** 웨이퍼 4장 중 하나에 의도적으로 장비 문제를 주입하고 판별기를 돌렸습니다.

  | 웨이퍼 | 주입한 상태 | 판정 결과 | |
  |---|---|---|---|
  | W01 | 정상 | 전건 PASS | ✅ |
  | W02 | 정상 (산포 약간 큼) | 전건 PASS | ✅ |
  | W03 | 변조 효율 열화 — **공정 이상** | PASS, **Cpk 0.28 지목** | ✅ |
  | W04 | 프로브 접촉 실패 — **장비 문제** | 해당 세션 **12건 DEAD** | ✅ |

  **주입한 12건만 정확히 검출 — 오탐 0건, 미탐 0건.** 나아가 **장비 문제(W04)와 공정 이상(W03)을 구분**해 냈습니다.
  단위 테스트는 **115건** 통과합니다.

- **Apache6 AI Model Benchmark Dashboard** &nbsp;`🔒 비공개 저장소 — 요청 시 공유` <br/>
| `Python` `FastAPI` `C++` `aiWare NPU` `YOLO11` `ONNX` `INT8 PTQ` `TCP` <br/>

  ### ▶ 데모 영상 (25초) — 이미지를 클릭하면 재생됩니다
  <a href="https://github.com/dw7566/dw7566/blob/main/assets/apache6/dashboard_menu_tour_30s.mp4">
    <img width="640" height="auto" alt="Apache6 Dashboard 데모 영상" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/apache6/tour_thumbnail.png" /> <br/>
  </a>

  Apache6(aiWare NPU) 보드에서 **YOLO 계열 모델을 실시간 구동·평가·비교하는 벤치마크 대시보드**와, 그 모델을 만드는 **YOLO11 + LKA 학습 → ONNX Export → NPU 컴파일 파이프라인**.

  <img width="640" height="auto" alt="실시간 검출 대시보드" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/apache6/dashboard_full_coco.png" />

  <sub>실시간 영상 탭 — NPU 추론시간 1.68 ms · NPU 온도 50.2 °C · 메모리 718 MB 전부 **실측**.
  이 외 벤치마크 · 결과(mAP50 · Confusion Matrix) · 설정 탭으로 구성됩니다.</sub>

  **측정으로 얻은 결과**

  | 항목 | 값 |
  |---|---|
  | NPU 순수 추론 (v8n_nc6_bdd 640×384) | **1.4 ms** |
  | 상주 런타임 도입 효과 | 프레임당 419 ms → 20.1 ms (**20.9배**) |
  | 프리뷰 전송량 | raw 230 KB → MJPEG 7.2 KB/frame (**1/31**) |
  | 검출 시간축 | 벽시계 고정 — **드리프트 0**, bbox 누적 지연 없음 |
  | GT 자기일치 검증 | recall 1.0 / mean IoU 0.9999 (파이프라인 결정론 확인) |

  - **아키텍처** — VM의 FastAPI 대시보드(27개 REST API) ↔ TCP 9000 JSON 라인 프로토콜 ↔ 보드 에이전트(Python) + `infer_frame`(C++, aiWare SDK 상주 추론)
  - **실시간성 설계** — 프리뷰는 `-re` 페이싱 MJPEG, 검출은 **drop-to-latest** 로 최신 프레임만 추론해 박스가 과거로 밀리지 않게 함
  - **커스텀 모델** — Ultralytics 8.3.240 포크에 `LKA` / `C2PSA_LKA` 블록 추가. 3×3 DW(d=1→2→3→4) → 1×1 PW 로 receptive field를 넓히되 **NPU 제약(max_dilation=5, max_window=17) 안에서** 동작하도록 설계
  - **정확도 분석** — BDD val 10k 대상 INT8 PTQ 캘리브레이션 비교(주간 전용 vs 혼합), 주야 도메인별·객체 크기별 성능 분석
  - 실보드 없이 도는 **테스트 스위트 17개**, 보드 미연결 시 결정론적 시뮬레이션으로 단독 기동

- **[Embedded SEM Defect AI >> https://github.com/dw7566/embedded-sem-defect-ai](https://github.com/dw7566/embedded-sem-defect-ai)** <br/>
| `C++` `TensorFlow Lite` `U-Net` `Mali GPU` `V4L2` `OpenGL ES` `APACHE6` <br/>
  <a href="https://github.com/dw7566/embedded-sem-defect-ai">
    <img width="512" height="auto" alt="GPU realtime demo" src="https://raw.githubusercontent.com/dw7566/embedded-sem-defect-ai/main/docs/images/demo-gpu-overlay.png" /> <br/>
  </a>
  - SEM 웨이퍼 결함 검사를 위한 **멀티태스크 U-Net 실시간 추론 엔진**
  - 6종 결함 분류 + 픽셀 단위 분할을 **한 번의 추론**으로 동시 수행
  - NextChip APACHE6 Mali GPU · TFLite GPU 델리게이트 · CPU 자동 폴백
  - 카메라 프레임 → 결함 종류 · 면적 비율 · PASS/FAIL 판정, plain C API로 노출

- **[picqa — Photonic IC Quality Analyzer >> https://github.com/dw7566/picqa](https://github.com/dw7566/picqa)** <br/>
| `Python` `Silicon Photonics` `CI` `CLI` `Curve Fitting` <br/>
  - 실리콘 포토닉스 웨이퍼 측정 데이터 분석 **모듈식 라이브러리 + CLI** — 709개 XML(484.8 MB)을 명령 한 번으로 처리
  - MZM 파라미터(FSR, |dλ/dV|, Peak IL, ER)와 **Vπ · Vπ·L** 추출
  - PN 변조기 **길이 의존성**(500/1500/2500 µm) 선형 피팅으로 µm당 도핑 손실 산출
  - 레이아웃·밴드 비의존 설계 — O/E/S/C/L/U 밴드 자동 판별, 임의 OIO XML 데이터셋 대응

- **[XML Analyzer >> https://github.com/dw7566/xml_analyzer_project](https://github.com/dw7566/xml_analyzer_project)** <br/>
| `Python` `GUI` `MZM` `IV Curve` `Excel` <br/>
  <a href="https://github.com/dw7566/xml_analyzer_project">
    <img width="512" height="auto" alt="XML Analyzer GUI" src="https://github.com/user-attachments/assets/0243242c-ec55-4afc-a7dd-82c6a0b21377" /> <br/>
  </a>
  - MZM 및 IV 커브 데이터 분석·시각화 **통합 GUI 애플리케이션**
  - Reference Spectrum 6차 다항식 피팅 → MZI 모델 커브 피팅 → 1550 nm 정규화
  - 다이오드 방정식 기반 Advanced IV Fitting, 파라미터 Excel 리포트 자동 생성
  - 수작업 분석 과정을 자동화해 처리 시간 단축

- **[Zonal Architecture Kit >> https://github.com/dw7566/zonal](https://github.com/dw7566/zonal)** <br/>
| `C` `C++` `Python` `FreeRTOS` `CAN` `Qt/QML` `NPU` `TOPST` <br/>
  <a href="https://github.com/dw7566/zonal">
    <img width="512" height="auto" alt="Zonal dashboard" src="https://raw.githubusercontent.com/dw7566/zonal/main/docs/dashboard_screenshot.png" /> <br/>
  </a>
  - TOPST 기반 **Zonal E/E 아키텍처** 교육 키트 — 축소 차량 제어·시각화 전체 파이프라인
  - AI 보드 NPU 추론 → AP 브리지 → Zone 컨트롤러(MCU) / 클러스터 디스플레이로 역할 분배
  - 표시용 / 제어용 **homography를 분리한 dual-BEV** 변환
  - FreeRTOS 펌웨어로 모터·조향·조명·CAN 통신 담당, Qt/QML로 3D BEV·ADAS 경고 UI

- **[Elliott Wave Trading Bot >> https://github.com/dw7566/Automated-Trading-bot](https://github.com/dw7566/Automated-Trading-bot)** <br/>
| `Python` `Backtesting` `Signal Processing` <br/>
  - 엘리어트 파동이론의 **규칙을 코드로 매핑**한 파동 인식 + 매매 신호 + 백테스트 봇
  - 모노웨이브 분리(ZigZag 피벗) → 충격파 3대 절대 규칙 검증 → 카운트 신뢰도 점수화
  - **규칙 위반 가격 = 카운트 무효화 = 손절가**로 이론과 리스크 관리를 일치시킴

- **[전체 레포지토리 https://github.com/dw7566?tab=repositories](https://github.com/dw7566?tab=repositories)**
