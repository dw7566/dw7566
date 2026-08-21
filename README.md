# 이재혁 · Jaehyeok Lee

AI를 도구로 써서 **가치 있는 것을 만들고, 귀찮은 일을 없애고, 느린 것을 빠르게** 합니다.
손으로 열 수 없던 계측 XML 709개를 명령 한 번으로 처리하고, 엣지 추론을 프레임당
419 ms에서 20.1 ms로 줄이고, 불확실성 기반 판정으로 실계측 76.1%를 생략하게 만든
작업들이 그 결과입니다.

도메인은 가리지 않습니다. 반도체 공정·계측을 가장 깊게 다루지만 실리콘 포토닉스,
디스플레이 공정, 엣지 AI·NPU, 차량 E/E 아키텍처까지 필요한 곳이면 들어갑니다.
그중 오래 파고든 주제는 계측 데이터의 **판정 기준**입니다 — 값을 예측하는 데서 끝내지 않고
불확실성을 함께 산출해 "실계측을 생략해도 되는가"까지 답하며, 그 판정기를 ARM64 보드
위에서 동작시킵니다.

한양대학교 ERICA 차세대반도체융합공학부 반도체디스플레이전공 · 팹리스점프업 2기
dw7566@hanyang.ac.kr

---

## 작업 방식 — 도메인을 깔고, 출력을 검증한다

> AI를 활용한다는 것은 답을 시키는 일이 아니라, 기본 도메인을 정확히 깔아 주어 그 위에서 일하게 하는 일이다.
>
> — 「정답을 받는 대신, 도메인을 먼저 깔았다」 · 2026 ERICA人 AI 학습 활용 사례 공모전 최우수상

실리콘 포토닉스 웨이퍼 측정 데이터 709개 XML(4 웨이퍼 × 14 다이 × 13 사이트, 484.8 MB)을
분석하는 IC-PBL 과제에서 정립한 원칙입니다. 문제 정의·설계 판단·검증은 직접 수행하고,
반복 구현과 리팩터링은 위임합니다.

세 번의 실패가 기준을 만들었습니다.

| 상황 | 결과 |
|---|---|
| "이 데이터로 그래프를 그려 줘" | 곡선은 그럴듯했으나 물리적 의미가 실측과 달랐다. 간섭 투과 곡선의 식과 축의 물리량을 먼저 정의한 뒤에야 분석 가능한 출력이 나왔다 |
| 배경 제거 보정 제안을 수용 | 분석 대상인 핵심 특징까지 함께 제거됐다 |
| 곡선 피팅 초기값을 고정 | R²가 0에 가깝게 무너졌다. 데이터에서 초기값을 탐색하도록 바꾸자 0.95 |

출력이 그럴듯할수록 검증이 필요하다는 결론이고, 저장소마다 그 흔적을 남깁니다.

| 원칙 | 구현된 증거 |
|---|---|
| 변하지 않아야 할 것이 변하지 않았음을 보인다 | `plasma-etch` — 챔버 압력 88장 전체 표준편차 0.0000 (음성 대조군) |
| 정답을 아는 데이터로 시험한다 | `WaferSense` — 이상 주입 검증에서 오탐 0건·미탐 0건 |
| 재현 가능한 형태로 남긴다 | 외부 데이터 없이 도는 테스트 115 · 47 · 27 · 17건 |
| 비율에는 신뢰구간을 병기한다 | 미검출 0/67을 "0%"가 아니라 "95% 상한 5.4%"로 기술 |

---

## Selected Work

| 프로젝트 | 영역 | 핵심 지표 |
|---|---|---|
| [Plasma Etch Virtual Metrology](https://github.com/dw7566/plasma-etch-virtual-metrology) | 가상계측 · 임베디드 | LOLO R² 0.855 · 실계측 76.1% 생략 · 미검출 0/67 |
| [WaferSense](https://github.com/dw7566/wafersense) | 계측 신뢰성 · 공정능력 | 무효 12건 자동 격리 · 주입 검증 오탐·미탐 0 |
| Apache6 Benchmark Dashboard 🔒 | NPU 추론 · 벤치마크 | NPU 추론 1.4 ms · 상주 런타임 20.9× |
| [Embedded SEM Defect AI](https://github.com/dw7566/embedded-sem-defect-ai) | 엣지 AI · 결함 검사 | 분류+분할 단일 추론 · GPU 171.9 ms |
| [picqa](https://github.com/dw7566/picqa) | 실리콘 포토닉스 분석 | 709 XML end-to-end · 테스트 47건 · CI |
| [Zonal Architecture Kit](https://github.com/dw7566/zonal) | 분산 E/E 아키텍처 | NPU → AP → MCU/클러스터 파이프라인 |

---

### Plasma Etch Virtual Metrology

[`plasma-etch-virtual-metrology`](https://github.com/dw7566/plasma-etch-virtual-metrology) ·
Python · C · scikit-learn · ARM64 · System V IPC

<img width="560" alt="식각 깊이 드리프트" src="https://raw.githubusercontent.com/dw7566/plasma-etch-virtual-metrology/main/figs/fig1_drift.png" />

BOSCH 식각 공정의 챔버 드리프트를 인과까지 규명하고, 센서 3개로 공정 시작 4.8초 시점에
식각 깊이를 예측하는 판정기를 임베디드 보드에 실장했습니다.
원저자가 inconclusive로 남긴 문제를 분석 대상으로 삼았습니다.

- 드리프트 −0.119 µm/wafer, 10개 lot 전부 단조 감소 (p = 4.5×10⁻¹⁹)
- 원인은 RF 정합 특성 변화. 압력·설정파워는 변화 0.000으로 음성 대조군 성립
- 5 특징셋 × 13 모델 = 65조합을 Leave-One-Lot-Out으로 전수 비교
- 불확실성 기반 3분기 판정으로 실계측 76.1% 생략, 규격이탈 미검출 0/67 (95% 상한 5.4%)
- C 586줄로 ARM64 이식 — PC 대비 오차 1.0×10⁻⁵ µm, 누적 샘플 88/88 일치
- OES 3,648채널에서 543–562 nm 대역 감소를 4개 lot 전부 재현 (p < 0.02)

가설을 세 번 기각한 과정과 검증 불가능한 항목을 문서에 그대로 남겼습니다.

---

### WaferSense

[`wafersense`](https://github.com/dw7566/wafersense) ·
Python · Cpk · Streamlit · Tkinter · LLM · PyInstaller

<a href="https://github.com/dw7566/dw7566/blob/main/assets/wafersense/wafersense_demo.mp4">
  <img width="560" alt="판별 결과 — 시연 영상 재생" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/wafersense/verdict_summary.png" />
</a>

<sub>▶ 이미지를 클릭하면 시연 영상(71초)이 재생됩니다.</sub>

계측 데이터 분석의 첫 병목은 값을 읽는 일이 아니라 어떤 측정을 믿을 수 있는지 가려내는
일입니다. 프로브 접촉 불량이나 장비 오작동으로 생긴 무효 측정이 통계에 섞이면 공정 판단
자체가 오염되기 때문입니다.

- 모든 측정에 `PASS` / `SUSPECT` / `DEAD` 등급과 사유를 기록. 데이터를 삭제하지 않으므로 필터링 자체가 감사 가능
- 웨이퍼 × 밴드별 NU · CV · Cpk 산출로 규격 내 산포 확대를 포착하고 최취약 항목을 지목
- 다중 LLM 리포트 — 키 접두사 자동 인식, 모델 사용 중단(404) 시 다음 세대로 자동 재시도
- CLI · 데스크톱 GUI · 웹 GUI 세 경로와 Windows 실행파일 빌드

**검증 설계.** 웨이퍼 4장 중 하나에 의도적으로 장비 문제를 주입하고 판별기를 돌렸습니다.

<img width="560" alt="정답 주입 검증 결과" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/wafersense/validation.png" />

| 웨이퍼 | 주입한 상태 | 판정 |
|---|---|---|
| W01 | 정상 | 전건 PASS |
| W02 | 정상 (산포 약간 큼) | 전건 PASS |
| W03 | 변조 효율 열화 — 공정 이상 | PASS, Cpk 0.28 지목 |
| W04 | 프로브 접촉 실패 — 장비 문제 | 해당 세션 12건 DEAD |

주입한 12건만 정확히 검출했고 오탐·미탐이 없었으며, 장비 문제와 공정 이상을 구분했습니다.
단위 테스트 115건이 통과합니다.

---

### Apache6 AI Model Benchmark Dashboard

비공개 저장소 (요청 시 공유) ·
Python · FastAPI · C++ · aiWare NPU · YOLO11 · ONNX · INT8 PTQ

<a href="https://github.com/dw7566/dw7566/blob/main/assets/apache6/dashboard_menu_tour_30s.mp4">
  <img width="560" alt="대시보드 데모 영상" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/apache6/tour_thumbnail.png" />
</a>

<sub>▶ 이미지를 클릭하면 데모 영상(25초)이 재생됩니다.</sub>

Apache6(aiWare NPU) 보드에서 YOLO 계열 모델을 실시간 구동·평가·비교하는 벤치마크
대시보드와, 그 모델을 만드는 학습 → ONNX Export → NPU 컴파일 파이프라인입니다.

<img width="560" alt="실시간 검출 화면" src="https://raw.githubusercontent.com/dw7566/dw7566/main/assets/apache6/dashboard_full_coco.png" />

<sub>실시간 영상 탭. NPU 추론시간·온도·메모리는 모두 실측값입니다.</sub>

- VM의 FastAPI 대시보드(27 REST API) ↔ TCP JSON 라인 프로토콜 ↔ 보드 에이전트 + C++ 상주 추론
- 상주(serve) 런타임 전환으로 프레임당 419 ms → 20.1 ms (20.9배)
- `-re` 페이싱 MJPEG 프리뷰로 전송량 1/31, drop-to-latest로 검출 시간축 드리프트 0
- Ultralytics에 LKA / C2PSA_LKA 블록 이식 — NPU 제약(max_dilation 5, max_window 17) 안에서 receptive field 확장
- BDD val 10k 대상 INT8 PTQ 캘리브레이션 비교, 주야 도메인·객체 크기별 정확도 분석
- GT 자기일치 검증 recall 1.0 / mean IoU 0.9999, 실보드 없이 도는 테스트 17개

---

### Embedded SEM Defect AI

[`embedded-sem-defect-ai`](https://github.com/dw7566/embedded-sem-defect-ai) ·
C++ · TensorFlow Lite · U-Net · Mali GPU · V4L2 · OpenGL ES

<img width="560" alt="GPU 실시간 데모" src="https://raw.githubusercontent.com/dw7566/embedded-sem-defect-ai/main/docs/images/demo-gpu-overlay.png" />

SEM 웨이퍼 결함 검사를 위한 멀티태스크 U-Net 추론 엔진입니다. 6종 결함 분류와 픽셀 단위
분할을 한 번의 추론으로 동시에 수행해 카메라 프레임에서 결함 종류·면적 비율·PASS/FAIL을
산출합니다. NextChip APACHE6의 Mali GPU에서 TFLite GPU 델리게이트로 구동하며 CPU
폴백을 갖추고, C 카메라 루프에 임베드할 수 있도록 plain C API로 노출했습니다.

---

### picqa — Photonic IC Quality Analyzer

[`picqa`](https://github.com/dw7566/picqa) · Python · CLI · CI

실리콘 포토닉스 웨이퍼 측정 데이터를 분석하는 모듈식 라이브러리와 CLI입니다.
709개 XML을 명령 한 번으로 처리합니다.

- 레이아웃·밴드 비의존 설계 — 폴더 구조 자동 판별, O/E/S/C/L/U 밴드 자동 인식
- MZ 변조기 · PN 변조기 · 광검출기 · 도파로 소자별 특성 추출기
- MZM: FSR, |dλ/dV|, Peak IL, ER / 위상 분석에서 Vπ, Vπ·L 도출
- PN 길이 의존성(500 / 1500 / 2500 µm) 선형 피팅으로 µm당 도핑 손실 산출

[`xml_analyzer_project`](https://github.com/dw7566/xml_analyzer_project)는 같은 과제의 초기
GUI 버전으로, 교수 피드백을 거쳐 코드로 불러 쓰고 검증할 수 있는 모듈형 라이브러리로
재설계한 결과가 picqa입니다.

---

### Zonal Architecture Kit

[`zonal`](https://github.com/dw7566/zonal) · C · C++ · Python · FreeRTOS · CAN · Qt/QML

<img width="560" alt="클러스터 대시보드" src="https://raw.githubusercontent.com/dw7566/zonal/main/docs/dashboard_screenshot.png" />

하나의 SoC가 모든 것을 처리하는 대신 역할이 분리된 노드가 네트워크로 통신하는 Zonal
아키텍처를 축소 차량으로 재현한 교육 키트입니다. AI 보드가 NPU로 추론하고, AP 브리지가
표시용·제어용 homography를 분리해 dual-BEV로 가공하며, Zone 컨트롤러(FreeRTOS)가
모터·조향·조명·CAN을 담당하고, 클러스터(Qt/QML)가 3D BEV와 ADAS 경고를 표시합니다.

---

## 기술 스택

| 영역 | 내용 |
|---|---|
| 언어 | Python, C (C99), C++ |
| 데이터 분석 | NumPy · pandas · SciPy · scikit-learn · netCDF4 · Matplotlib |
| 통계적 판정 | Leave-One-Lot-Out 교차검증, Gaussian Process, leverage 기반 예측 분산, Wilson 신뢰구간, Cpk / NU / CV |
| 임베디드 | ARM64 크로스컴파일(Cortex-A65AE), System V IPC, V4L2 / MIPI CSI, Wayland / OpenGL ES, FreeRTOS, CAN |
| 엣지 AI | TensorFlow Lite GPU 델리게이트, aiWare NPU SDK, ONNX, YOLO / Ultralytics, INT8 PTQ |
| 애플리케이션 | FastAPI, Streamlit, Tkinter, Qt/QML, PyInstaller |
| 공정·계측 도메인 | ICP-RIE 플라즈마 식각, Oxide TFT 공정, 실리콘 포토닉스, OES 발광분광, SEM 결함 검사 |
| 개발 환경 | Git · GitHub Actions, pytest, 크로스 플랫폼 빌드 |

수치 안정성처럼 눈에 띄지 않는 문제도 다룹니다. 예측식을 원시입력 형태로 두면 float32
파괴적 상쇄로 0.0486 µm 오차가 생기는데, 표준화 형태와 double 누적으로 9.9×10⁻⁶ µm까지
줄였습니다.

---

## 이력

| 시기 | 내용 |
|---|---|
| 2026.07 | **2026 ERICA人 AI 학습 활용 사례 공모전 최우수상** — 한양대학교 ERICA IC-PBL교수학습센터 |
| 2026.08 | 디스플레이 부트캠프 — OLED 공정 및 XR 현장 실습 (충남테크노파크 혁신공정센터) |
| 2026.06 | 디스플레이 부트캠프 — 박막트랜지스터 공정 실습 |
| 2026.05 | 현직자 직무부트캠프 G7/GX 이수 — ㈜월드클래스에듀케이션 |

### 박막트랜지스터 공정 실습

Oxide TFT(Bottom Gate 구조)를 직접 제작하며 반도체 8대 공정을 실습했습니다.
데이터로만 다루던 공정을 직접 수행한 경험이 공정 데이터를 해석하는 근거가 됩니다.

| 단계 | 공정 | 재료 |
|---|---|---|
| 1 | 기판 세정 | Si / SiO₂ |
| 2 | Gate 패터닝 | Mo |
| 3 | Insulator 증착 (ALD) | Al₂O₃ |
| 4 | Channel 증착 및 패터닝 | ITZO |
| 5 | Source/Drain 패터닝 | Mo |

포토리소그래피 마스크 얼라인으로 미세 구조를 형성하는 과정을 직접 수행했고,
OLED 현장 실습에서는 검·계측 장비와 SAICAS 박막 스크래치 분석 사례를 통해
측정 결과가 공정 평가로 이어지는 흐름을 확인했습니다.

---

[전체 저장소](https://github.com/dw7566?tab=repositories)
