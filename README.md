## 이미지 필터링 도구

### 1. 프로젝트 소개

**프로젝트 이름**: 의료 이미지 필터링 도구

**목적**: 의료 이미지를 필터링하고 개선하여 시각적으로 더 명확하게 만드는 도구를 개발합니다.

**주요 기능**:

- 의료 이미지 불러오기 (예: 초음파 이미지)
- 이미지 필터링 (예: Gaussian 블러, Sharpen 필터)
- 필터 적용 전후 비교 시각화
- 결과 이미지 저장

**기술 스택**: Python, OpenCV, NumPy, Matplotlib, Pillow

---

### 2. 디렉토리 구조 설정

```python
medical_image_filtering/
│
├── data/
│   └── ultrasound_image.gif  # 예제 이미지 파일
│
├── src/
│   └── filter.py  # 이미지 필터링 코드
│
└── venv/  # 가상 환경 디렉토리
```

1. **이미지 파일 추가**:
    - Free Photo of Ultrasound Testing에서 이미지를 다운로드하여 `ultrasound_image.gif`로 저장합니다.
    - 저장 위치를 `medical_image_filtering/data` 디렉토리로 지정합니다.

---

### 3. 코드 작성

**filter.py**:

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import os

def load_image(file_path):
    """
    Load an image from a file.
    """
    if not os.path.exists(file_path):
        raise FileNotFoundError(f"The file {file_path} does not exist.")

    pil_image = Image.open(file_path).convert('L')  # Convert to grayscale
    image = np.array(pil_image)

    if image is None:
        raise ValueError(f"Failed to load the image from {file_path}.")

    return image

def apply_filters(image):
    """
    Apply Gaussian blur and sharpen filters to the image.
    """
    # Gaussian blur
    blurred_image = cv2.GaussianBlur(image, (5, 5), 0)

    # Sharpen filter
    kernel = np.array([[0, -1, 0], [-1, 5, -1], [0, -1, 0]])
    sharpened_image = cv2.filter2D(image, -1, kernel)

    return blurred_image, sharpened_image

def visualize_images(original, blurred, sharpened):
    """
    Visualize the original, blurred, and sharpened images.
    """
    plt.figure(figsize=(15, 5))

    plt.subplot(1, 3, 1)
    plt.imshow(original, cmap='gray')
    plt.title('Original Image')
    plt.axis('off')

    plt.subplot(1, 3, 2)
    plt.imshow(blurred, cmap='gray')
    plt.title('Blurred Image')
    plt.axis('off')

    plt.subplot(1, 3, 3)
    plt.imshow(sharpened, cmap='gray')
    plt.title('Sharpened Image')
    plt.axis('off')

    plt.tight_layout()
    plt.savefig('image_filtering.png')
    plt.show()

if __name__ == "__main__":
    # Load the image
    image_path = '/Users/jamie/PycharmProjects/medical_image_filtering/data/ultrasound_image.gif'
    image = load_image(image_path)

    # Apply filters
    blurred_image, sharpened_image = apply_filters(image)

    # Visualize the images
    visualize_images(image, blurred_image, sharpened_image)

```

---

### 4. 문제 해결 과정

코드를 실행하는 동안 이미지 파일을 불러오는 데 문제가 발생했습니다. 구체적으로 `cv2.GaussianBlur` 함수에 전달된 이미지가 비어 있어 오류가 발생했습니다. 이 문제를 해결하기 위해 다음 단계를 수행했습니다:

1. **파일 경로 확인**:
    - 이미지 파일의 절대 경로를 확인했습니다.
    - 이미지 파일이 존재하는지 확인했습니다.
2. **Pillow 사용**:
    - OpenCV가 `.gif` 파일을 제대로 읽지 못하는 문제를 해결하기 위해 `Pillow` 라이브러리를 사용했습니다.
    - `Pillow`로 이미지를 읽고, 이를 OpenCV 형식으로 변환한 후 필터를 적용했습니다.

최종적으로 코드가 성공적으로 실행되었고, 결과 이미지를 얻을 수 있었습니다.

---

### 5. 결과 확인

1. **결과 이미지 확인**:
    - 코드 실행 후, 프로젝트 루트 디렉토리에 `image_filtering.png` 파일이 생성되었는지 확인합니다.
    - `image_filtering.png` 파일을 열어 시각화된 필터 적용 전후의 이미지를 확인합니다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f9f35de7-0091-4a79-819a-501ef9435828/36478cad-66dc-4379-b8c9-1b95399370f8/Untitled.png)
    

---

### 6. 프로젝트 요약 및 결론

**프로젝트 요약**:
이 프로젝트는 의료 이미지를 필터링하고 개선하여 시각적으로 더 명확하게 만드는 도구를 개발하였습니다. OpenCV를 사용하여 이미지를 로드하고 필터를 적용하였으며, Matplotlib을 사용하여 결과 이미지를 시각화하였습니다. 또한, 이미지 파일을 제대로 불러오지 못하는 문제를 해결하기 위해 Pillow 라이브러리를 사용하였습니다.

**결론**:
이 프로젝트를 통해 이미지 처리 기본기를 향상시킬 수 있었으며, 이미지의 품질을 개선하여 더 나은 진단 도구를 구현할 수 있는 기술을 연습했습니다.
