## 1. 구글 애드센스 계정 생성 및 광고 단위 만들기

구글 애드센스에 가입합니다. 이미 계정이 있다면 로그인합니다. 애드센스 대시보드에서 "광고 단위 만들기"를 클릭하여 새 광고 단위를 만듭니다. 광고 단위의 설정을 완료하고 광고 코드를 복사합니다.

## 2. Hugo Doks 테마에 애드센스 코드 추가하기

### 2.1 헤더에 애드센스 스크립트 추가

Hugo 사이트의 루트 디렉토리에서 `layouts/partials` 폴더로 이동합니다. 만약 이 폴더가 없으면 생성합니다. `head.html` 파일을 열어 다음의 애드센스 스크립트를 추가합니다. 이 파일이 없다면 새로 만듭니다.

```html
<!-- layouts/partials/head.html -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
    (adsbygoogle = window.adsbygoogle || []).push({
        google_ad_client: "YOUR_AD_CLIENT_ID",
        enable_page_level_ads: true
    });
</script>
```

여기서 `YOUR_AD_CLIENT_ID`를 자신의 애드센스 계정에서 받은 ID로 교체합니다.

### 2.2 본문에 광고 단위 삽입

광고를 본문에 삽입하려면 적절한 위치에 광고 코드를 추가해야 합니다. 예를 들어, 블로그 포스트 본문에 광고를 넣으려면 `single.html` 파일을 수정해야 합니다.

`layouts/_default/single.html` 파일을 열어 원하는 위치에 광고 코드를 추가합니다. 보통 콘텐츠 중간이나 끝에 넣습니다.

```html
<!-- layouts/_default/single.html -->
<article>
    {{ .Content }}
    <!-- 광고 코드 삽입 -->
    <ins class="adsbygoogle"
         style="display:block"
         data-ad-client="YOUR_AD_CLIENT_ID"
         data-ad-slot="YOUR_AD_SLOT_ID"
         data-ad-format="auto"></ins>
    <script>
         (adsbygoogle = window.adsbygoogle || []).push({});
    </script>
</article>
```

여기서 `YOUR_AD_CLIENT_ID`와 `YOUR_AD_SLOT_ID`를 자신의 애드센스 계정에서 받은 ID로 교체합니다.

## 3. Hugo 사이트 재배포

변경 사항을 저장한 후 Hugo 사이트를 재배포합니다.