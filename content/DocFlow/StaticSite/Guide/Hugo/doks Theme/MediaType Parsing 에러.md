## 에러 내용

```sh
Built in 527 ms
Error: error building site: render: failed to render pages: render of "page" failed: "D:\Obsidian\DocFlow_Hugo\node_modules\@hyas\doks-core\layouts\_default\single.html:39:36": execute of template failed: template: _default/single.html:39:36: executing "main" at <.Content>: error calling Content: "D:\Obsidian\DocFlow\About\About.md:1:1": "D:\Obsidian\DocFlow_Hugo\node_modules\@hyas\images\layouts\_default\_markup\render-image.html:155:12": execute of template failed: template: _default/_markup/render-image.html:155:12: executing "_default/_markup/render-image.html" at <$r.MediaType.SubType>: can't evaluate field MediaType in type string
```

## 발생 원인

템플릿 렌더링 중 `MediaType` 필드를 평가할 수 없다는 내용이 있습니다. 이는 `resources.Get` 함수가 리소스를 찾지 못했거나, 찾은 리소스의 형식이 예상한 대로 `MediaType` 필드를 가지지 않는 경우 발생할 수 있습니다.

## 해결 방안

### HUGO 버전 확인
`render-image.html` 파일에서는 Hugo 버전이 최소 `0.114.0` 이상이어야 한다고 명시되어 있습니다. 사용 중인 Hugo 버전이 해당 버전 이상인지 확인합니다.

```sh
hugo version
hugo v0.127.0-74e0f3bd63c51f3c7a0f07a7c779eec9e922957e+extended windows/amd64 BuildDate=2024-06-05T10:27:59Z VendorInfo=gohugoio
```

### 템플릿 디버깅
`render-image.html` 파일에서 `$r` 변수가 예상대로 설정되고 있는지 확인합니다. `$r` 변수가 제대로 설정되지 않으면 `MediaType` 필드를 접근할 수 없습니다

- `node_modules/@hyas/images/layouts/_default/_markup/render-image.html`

```html
{{ $renderHookName := "image" }}
{{ $minHugoVersion := "0.114.0" }}
{{ if lt hugo.Version $minHugoVersion }}
  {{ errorf "The %q render hook requires Hugo v%s or later." $renderHookName $minHugoVersion }}
{{ end }}
{{ $errorLevel := or site.Params.render_hooks.image.errorLevel "ignore" | lower }}
{{ if not (in (slice "ignore" "warning" "error") $errorLevel) }}
  {{ errorf "The %q render hook is misconfigured. The errorLevel %q is invalid. Please check your site configuration." $renderHookName $errorLevel }}
{{ end }}
{{ $contentPath := "" }}
{{ with .Page.File }}
  {{ $contentPath = .Path }}
{{ else }}
  {{ $contentPath = .Path }}
{{ end }}
{{ $u := urls.Parse .Destination }}
{{ $msg := printf "The %q render hook was unable to resolve the destination %q in %s" $renderHookName $u.String $contentPath }}
{{ $r := "" }}
{{ if $u.IsAbs }}
  {{ with resources.GetRemote $u.String }}
    {{ with .Err }}
      {{ if eq $errorLevel "warning" }}
        {{ warnf "%s. See %s" . $contentPath }}
      {{ else if eq $errorLevel "error" }}
        {{ errorf "%s. See %s" . $contentPath }}
      {{ end }}
    {{ else }}
      {{ $r = . }}
    {{ end }}
  {{ else }}
    {{ if eq $errorLevel "warning" }}
      {{ warnf $msg }}
    {{ else if eq $errorLevel "error" }}
      {{ errorf $msg }}
    {{ end }}
  {{ end }}
{{ else }}
  {{ with .Page.Resources.Get (strings.TrimPrefix "./" $u.Path) }}
    {{ $r = . }}
  {{ else }}
    {{ with (and (ne .Page.BundleType "leaf") (.Page.CurrentSection.Resources.Get (strings.TrimPrefix "./" $u.Path))) }}
      {{ $r = . }}
    {{ else }}
      {{ with resources.Get $u.Path }}
        {{ $r = . }}
      {{ else }}
        {{ if eq $errorLevel "warning" }}
          {{ warnf $msg }}
        {{ else if eq $errorLevel "error" }}
          {{ errorf $msg }}
        {{ end }}
      {{ end }}
    {{ end }}
  {{ end }}
{{ end }}
{{ if and (not (eq $r "")) (in (slice "image/png" "image/jpeg" "image/webp") $r.MediaType.Type) }}
  {{ $id := printf "h-rh-i-%d" .Ordinal }}
  {{ with .Attributes.id }}
    {{ $id = . }}
  {{ end }}
  {{ if and (not (eq $r.MediaType.SubType "")) (ne $r.MediaType.SubType "gif") }}
    {{ $r = $r.Resize (printf "%dx%d webp" $r.Width $r.Height) }}
  {{ end }}
  <img
    src="{{ $r.RelPermalink }}"
    width="{{ string $r.Width }}"
    height="{{ string $r.Height }}"
    decoding="{{ site.Params.hyas_images.defaults.decoding }}"
    fetchpriority="{{ site.Params.hyas_images.defaults.fetchpriority }}"
    loading="{{ site.Params.hyas_images.defaults.loading }}"
    alt="{{ .PlainText }}"
    {{ with .Title }}title="{{ . }}"{{ end }}
    id="{{ $id }}"
  />
{{ else }}
  <img
    src="{{ .Destination }}"
    alt="{{ .PlainText }}"
    {{ with .Title }}title="{{ . }}"{{ end }}
  />
{{ end }}
```

### 리소스 형식 확인
`MediaType`이 설정된 리소스만을 다루도록 확인합니다. 현재 템플릿은 `image/png`, `image/jpeg`, `image/webp` 형식만을 허용하고 있습니다.