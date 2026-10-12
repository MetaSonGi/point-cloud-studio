# 포인트 클라우드 스튜디오

이미지·영상을 AI 깊이 추정(Depth Anything V2)으로 3D 포인트 클라우드로 변환·시각화·내보내기하는 웹앱.
모든 처리는 브라우저 안에서 실행됩니다 (서버 전송 없음).

## 사용법

1. **입력**: 이미지 업로드(PNG/JPG/WebP) / 영상 업로드(MP4/WebM) / 샘플 이미지 2종(인물·풍경, Canvas API로 직접 그림)
   - 영상은 12프레임 썸네일 필름스트립에서 사용할 프레임을 클릭 선택
2. **AI 깊이 분석**: "AI 깊이 분석 실행" 버튼 → 진행률 바 표시 → 깊이맵(그레이스케일) 미리보기
3. **포인트 클라우드 생성**: 포인트 크기 / 깊이 스케일 / 샘플링 간격(1·2·4px) / Z 반전 설정 후 생성
4. **3D 뷰어**: 마우스 드래그 회전·휠 줌·우클릭 이동, 자동 회전 토글, 배경 검정/흰색 토글, 원본/깊이맵/3D 탭 전환
5. **내보내기**: PLY(ASCII, `x y z r g b`) 다운로드, 깊이맵 PNG 다운로드

## 기술 스택

- [transformers.js v3](https://huggingface.co/docs/transformers.js) (ESM CDN) — `pipeline('depth-estimation', 'onnx-community/depth-anything-v2-small')`
- three.js 0.160.0 (importmap CDN) + OrbitControls
- 단일 `index.html`, 외부 빌드 불필요

## 제약 사항

- **첫 실행 시 약 100MB의 AI 모델을 다운로드**합니다 (인터넷 필수, 1회만 — 이후 브라우저 캐시 사용).
- 저사양 기기(CPU만)에서는 깊이 분석에 수십 초~수 분이 걸릴 수 있습니다.
- 포인트가 많을수록(1px 샘플링) 메모리 사용량이 큽니다. 느리면 샘플링 간격을 2px·4px로 올리세요.
- PLY는 ASCII 포맷이라 대용량 포인트에서는 파일이 큽니다. MeshLab·CloudCompare·Blender에서 열 수 있습니다.
- 영상 프레임 추출은 브라우저의 `<video>` 시킹을 사용하므로, 일부 코덱에서는 썸네일 생성이 느릴 수 있습니다.

## 실행

`index.html`을 브라우저에서 열거나 GitHub Pages로 배포하면 바로 사용 가능합니다.
(AI 모델 로드는 CDN이 필요하므로 오프라인에서는 동작하지 않습니다.)
