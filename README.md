# 이재혁 (Jaehyeok Lee)

> 안녕하세요! **계측 데이터를 믿을 수 있는 판정으로 바꾸고, 그 판정을 보드 위에서 돌리는** 반도체 데이터 엔지니어입니다.

# 🌟 About Me

🏫  한양대학교 ERICA 차세대반도체융합공학부 반도체디스플레이전공

🕍  **팹리스점프업 2기**

🏆  **2026 ERICA人 AI 학습 활용 사례 공모전 최우수상** (한양대학교 ERICA IC-PBL교수학습센터, 2026.07)

📧  **Email** | dw7566@hanyang.ac.kr

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
- **V4L2 / MIPI CSI 카메라 캡처**, **Wayland / OpenGL ES** 표시 경로를 다뤘습니다.
- **FreeRTOS · CAN** 기반 Zone 컨트롤러와 Qt/QML 클러스터를 연동한 경험이 있습니다.

## 엔지니어링 습관

- **테스트를 남깁니다** — 외부 데이터 없이 도는 검증을 붙여 저장소만 clone하면 핵심 주장이 재현되게 만듭니다.
- **재현 절차와 함정을 문서화합니다** — 실제로 저질렀던 실수(사전 복호 누락으로 R² 0.2 손실 등)를 그대로 적습니다.
- **한계를 먼저 씁니다** — 결론 내리지 못한 것은 내리지 못했다고 남깁니다.

---

# 🏅 교육·실습 이력

## 🏆 2026 ERICA人 AI 학습 활용 사례 공모전 — **최우수상**

`2026.07` · 한양대학교 ERICA IC-PBL교수학습센터

생성형 AI를 학습에 활용한 사례로 최우수상을 수상했습니다. 실제로 이 저장소들의 상당수가
**도메인 지식 + AI 페어 프로그래밍**으로 만들어졌으며, 설계 판단과 검증은 직접 수행하고
반복 구현을 위임하는 방식으로 생산성과 코드 품질을 함께 확보했습니다.

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

- **[Embedded SEM Defect AI >> https://github.com/dw7566/embedded-sem-defect-ai](https://github.com/dw7566/embedded-sem-defect-ai)** <br/>
| `C++` `TensorFlow Lite` `U-Net` `Mali GPU` `V4L2` `OpenGL ES` `APACHE6` <br/>
  <a href="https://github.com/dw7566/embedded-sem-defect-ai">
    <img width="512" height="auto" alt="GPU realtime demo" src="https://raw.githubusercontent.com/dw7566/embedded-sem-defect-ai/main/docs/images/demo-gpu-overlay.png" /> <br/>
  </a>
  - SEM 웨이퍼 결함 검사를 위한 **멀티태스크 U-Net 실시간 추론 엔진**
  - 6종 결함 분류 + 픽셀 단위 분할을 **한 번의 추론**으로 동시 수행
  - NextChip APACHE6 Mali GPU · TFLite GPU 델리게이트 · CPU 자동 폴백
  - 카메라 프레임 → 결함 종류 · 면적 비율 · PASS/FAIL 판정, plain C API로 노출

- **[WaferSense >> https://github.com/dw7566/wafersense](https://github.com/dw7566/wafersense)** <br/>
| `Python` `Cpk` `Streamlit` `Tkinter` `LLM` `pytest` <br/>
  - 웨이퍼 계측 데이터 **신뢰성 판별 + 공정능력 자동 분석** 시스템
  - 무효 측정(dead run)을 `PASS`/`SUSPECT`/`DEAD` 3등급으로 판별 — **데이터를 삭제하지 않아 필터링 자체가 감사 가능**
  - 웨이퍼 × 밴드별 NU/CV/Cpk 산출로 규격 내 산포 확대(열화) 신호 포착
  - CLI · 데스크톱 GUI · 웹 GUI 세 경로 + Windows `.exe` 빌드, **테스트 115건**

- **[picqa — Photonic IC Quality Analyzer >> https://github.com/dw7566/picqa](https://github.com/dw7566/picqa)** <br/>
| `Python` `Silicon Photonics` `CI` `CLI` `Curve Fitting` <br/>
  - 실리콘 포토닉스 웨이퍼 측정 데이터 분석 **모듈식 라이브러리 + CLI**
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
