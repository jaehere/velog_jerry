<p>RGB는 <strong>Red(빨강), Green(초록), Blue(파랑)</strong>의 약자로, 디지털 화면에서 색을 표현하는 기본 방식 이다.</p>
<h2 id="rgb24와-rgb32">RGB24와 RGB32</h2>
<h3 id="rgb24">RGB24</h3>
<pre><code>•    총 24비트를 사용해 색 표현
•    Red, Green, Blue 각각 8비트씩 사용 → 각각 256단계(0~255) 색상 표현 가능
•    예: RGB(255, 0, 0) → 강한 빨간색</code></pre><h3 id="rgb32-rgba">RGB32 (RGBA)</h3>
<pre><code>•    RGB24에 8비트를 추가한 색 표현 방식
•    추가된 8비트는 투명도(Alpha)를 나타냄
•    Alpha = 255 → 완전히 불투명
•    Alpha = 0 → 완전히 투명</code></pre><h3 id="이미지-포맷-지원-차이">이미지 포맷 지원 차이</h3>
<p>우리가 자주 사용하는 JPEG는 RGB32를 지원하지 않는다.
투명한 이미지는 RGB32를 지원하는 PNG파일로 저장해야 한다.</p>
<pre><code>•    JPEG: RGB32(투명도)를 지원하지 않음 → 투명 배경 불가능
•    PNG: RGB32(투명도)를 지원 → 투명 이미지 저장 가능</code></pre>