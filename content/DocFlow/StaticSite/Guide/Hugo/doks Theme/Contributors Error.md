```sh
ERROR render of "term" failed: "D:\Obsidian\DocFlow_Hugo\node_modules\@hyas\doks-core\layouts\_default\term.html:47:15": execute of template failed: template: _default/term.html:47:15: executing "main" at <partial "main/blog-meta.html" .>: error calling partial: "D:\Obsidian\DocFlow_Hugo\node_modules\@hyas\doks-core\layouts\partials\main\blog-meta.html:1:17": execute of template failed: template: partials/main/blog-meta.html:1:17: executing "partials/main/blog-meta.html" at <len .Params.contributors>: error calling len: reflect: call of reflect.Value.Type on zero Value
Total in 1555 ms
Error: error building site: render: failed to render pages: render of "term" failed: "D:\Obsidian\DocFlow_Hugo\node_modules\@hyas\doks-core\layouts\_default\term.html:47:15": execute of template failed: template: _default/term.html:47:15: executing "main" at <partial "main/blog-meta.html" .>: error calling partial: "D:\Obsidian\DocFlow_Hugo\node_modules\@hyas\doks-core\layouts\partials\main\blog-meta.html:1:17": execute of template failed: template: partials/main/blog-meta.html:1:17: executing "partials/main/blog-meta.html" at <len .Params.contributors>: error calling len: reflect: call of reflect.Value.Type on zero Value
```

이 오류는 Hugo가 `contributors` 필드의 값을 처리할 때 발생합니다. 
- `contributors` 필드가 빈 값이거나 정의되지 않은 경우 발생하는 문제입니다.
- 메타데이터 템플릿을 수정하여 `contributors` 필드를 빈 배열로 초기화하여 해결할 수 있습니다.
	- 템플릿에서 `contributors` 필드를 처리할 때, 이 필드가 비어 있거나 존재하지 않는 경우를 안전하게 처리하도록 변경합니다.
	- `/node_modules/@hyas/doks-core/layouts/partials/main/blog-meta.html` 파일을 다음과 같이 수정합니다.

```html
{{ if isset .Params "contributors" }}
  {{ $contributors := .Params.contributors }}
  {{ if $contributors }}
    {{ $len := len $contributors }}
    {{ $last := sub $len 1 -}}
    <p>
      <small>
        {{- time.Format (default ":date_long" .Site.Params.dateFormat) .PublishDate -}}
        {{- with .Params.categories -}}
          &nbsp;in&nbsp;
          {{- range $index, $category := . -}}
            {{ if gt $index 0 }}, {{ end -}}
            <a class="stretched-link position-relative link-muted" href="{{ "/categories/" | relLangURL }}{{ . | urlize }}/">{{ . }}</a>
          {{- end }}
        {{- end }}
        {{- if gt $len 0 }}
          &nbsp;by&nbsp;
          {{- range $index, $contributor := $contributors -}}
            {{- if gt $index 0 }}{{ if eq $index $last }} and {{ else }}, {{ end }}{{ end -}}
            {{- with $.Site.GetPage "taxonomyTerm" (printf "contributors/%s" (urlize .)) -}}
              {{ if $.Params.avatar -}}
                {{ $image := .Resources.GetMatch (printf "**%s" .Params.avatar) -}}
                {{ $imageLq := $image.Resize "15x15 webp q95" -}}
                {{ $image = $image.Resize "60x60 webp q95" -}}
                <img class="rounded-circle w-auto mx-1 lazyload blur-up" src="{{ $imageLq.RelPermalink }}" data-src="{{ $image.RelPermalink }}" alt="{{ .Title }}" width="30" height="30">
              {{- end }}
            {{- end -}}
            <a class="stretched-link position-relative" href="{{ "/contributors/" | relLangURL }}{{ . | urlize }}/">{{ . }}</a>
          {{- end }}
        {{- end }}
        {{- /* NOTE: classes 'stretched-link position-relative' are necessary to properly display the title attribute on hover */ -}}
        <span class="stretched-link position-relative reading-time text-nowrap" title="{{ i18n "reading_time" }}">{{/* trim subsequent whitespace */ -}}
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-clock" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M3 12a9 9 0 1 0 18 0a9 9 0 0 0 -18 0"></path>
            <path d="M12 7v5l3 3"></path>
          </svg>
          {{- .ReadingTime }}&nbsp;{{ i18n "minute" .ReadingTime -}}
        </span>{{/* trim subsequent whitespace */ -}}
      </small>
    </p>
  {{ else }}
    <p>
      <small>
        {{- time.Format (default ":date_long" .Site.Params.dateFormat) .PublishDate -}}
        {{- with .Params.categories -}}
          &nbsp;in&nbsp;
          {{- range $index, $category := . -}}
            {{ if gt $index 0 }}, {{ end -}}
            <a class="stretched-link position-relative link-muted" href="{{ "/categories/" | relLangURL }}{{ . | urlize }}/">{{ . }}</a>
          {{- end }}
        {{- end }}
        {{- /* NOTE: classes 'stretched-link position-relative' are necessary to properly display the title attribute on hover */ -}}
        <span class="stretched-link position-relative reading-time text-nowrap" title="{{ i18n "reading_time" }}">{{/* trim subsequent whitespace */ -}}
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-clock" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M3 12a9 9 0 1 0 18 0a9 9 0 0 0 -18 0"></path>
            <path d="M12 7v5l3 3"></path>
          </svg>
          {{- .ReadingTime }}&nbsp;{{ i18n "minute" .ReadingTime -}}
        </span>{{/* trim subsequent whitespace */ -}}
      </small>
    </p>
  {{ end }}
{{ else }}
  <p>
    <small>
      {{- time.Format (default ":date_long" .Site.Params.dateFormat) .PublishDate -}}
      {{- with .Params.categories -}}
        &nbsp;in&nbsp;
        {{- range $index, $category := . -}}
          {{ if gt $index 0 }}, {{ end -}}
          <a class="stretched-link position-relative link-muted" href="{{ "/categories/" | relLangURL }}{{ . | urlize }}/">{{ . }}</a>
        {{- end }}
      {{- end }}
      {{- /* NOTE: classes 'stretched-link position-relative' are necessary to properly display the title attribute on hover */ -}}
      <span class="stretched-link position-relative reading-time text-nowrap" title="{{ i18n "reading_time" }}">{{/* trim subsequent whitespace */ -}}
        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-clock" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
          <path d="M3 12a9 9 0 1 0 18 0a9 9 0 0 0 -18 0"></path>
          <path d="M12 7v5l3 3"></path>
        </svg>
        {{- .ReadingTime }}&nbsp;{{ i18n "minute" .ReadingTime -}}
      </span>{{/* trim subsequent whitespace */ -}}
    </small>
  </p>
{{ end }}

```