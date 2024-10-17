<p>나중에 다른 글로 다루겠지만, 프론트의 로컬환경(http)에서 서버의 배포환경(http)과의 통신에서 쿠키 저장이 잘 안되는 사실을 알게 되어서, refresh token을 쿠키로 저장하는 이번 프로젝트에서 큰 문제로 인식했다.</p>
<p>해서 프론트와 백엔드 환경을 모두 https로 바꾸자는 결론이 났다.
이 글은 그 과정을 담은 글이다. </p>
<h2 id="그럼-http와-https의-차이는-무엇이냐">그럼 http와 https의 차이는 무엇이냐?</h2>
<p>한 마디로 하자면 s 붙은 게 좀 더 확장버전, 안전한 버전이라고 생각하면 좋다. HTTP는 암호화되지 않은 데이터를 전송하기 때문에 불안하다는 이유 때문. 해서 HTTPS 웹 사이트는 독립된 인증 기관(CA)에서 SSL/TLS 인증서를 획득해야 한다. </p>
<blockquote>
<ol>
<li>사용자 브라우저의 주소 표시줄에 https:// URL 형식을 입력하여 HTTPS 웹 사이트를 방문합니다.</li>
<li>브라우저는 서버의 SSL 인증서를 요청하여 사이트의 신뢰성을 검증하려고 시도합니다.
서버는 퍼블릭 키가 포함된 SSL 인증서를 회신으로 전송합니다.</li>
<li>웹 사이트의 SSL 인증서는 서버 아이덴티티를 증명합니다. 브라우저에서 인증되면, 브라우저가 퍼블릭 키를 사용하여 비밀 세션 키가 포함된 메시지를 암호화하고 전송합니다.</li>
<li>웹 서버는 개인 키를 사용하여 메시지를 해독하고 세션 키를 검색합니다. 그런 다음, 세션 키를 암호화하고 브라우저에 승인 메시지를 전송합니다.
<a href="https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/">출처 - http와 https 의 차이</a></li>
</ol>
</blockquote>
<h2 id="https-인증">https 인증</h2>
<p>그렇기 때문에 먼저 https 인증을 받아야했다. 이번에 이용한 것은 <strong>Let's Encrypt</strong>이라는 사용자에게 <strong>무료로 TLS 인증서를 발급</strong>해주는 비영리기관이다. 이 기관은 certbot이라는 인증서 발급 프로그램을 사용한다.</p>
<ol>
<li>도메인을 발급하고 설정한다.</li>
<li>웹 서버에 도메인과 관련된 설정을 해준다.(필자는 nginx를 이용한 http 배포를 먼저 진행했음.)</li>
<li>도메인에 대한 인증서를 cerbot으로 받는다. - 자동으로 nginx 설정에 인증서가 추가됨.</li>
<li>접속하면 끝!</li>
</ol>
<p>다소 애를 먹었던 것은, 리다이렉션에 대한 이해도가 좀 떨어져서 생긴 것인데,</p>
<pre><code>server {
        server_name 도메인;
        root /var/www/html;
        index index.html;

        location / {
                try_files $uri $uri/ /index.html;
        }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/도메인/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/도메인/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = 도메인) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


        server_name 도메인;
    listen 80;
    return 404; # managed by Certbot</code></pre><p>다음이 인증서 발급까지 완료된 nginx 설정이다. 코드 읽기 귀찮아서 안 읽고 그냥 막 접속해봤었는데, 분명 다 했는데 안 되는 것. 자세히 보니 listen 443 ssl 이 부분이 해결의 키였다.
내 서버에 대한 443 포트가 안 열려있었기 때문이었다. 보안 그룹에서 열어주니 접속이 잘 되기 시작했다.</p>
<p>+++ 추가 글로 다루려고 했는데 글의 양이 많지 않을 거 같아서 밑에 첨글한다.</p>
<p>https 인증으로 도메인까지 제대로 열게 된 나는 서둘러서 쿠키 설정 코드를 살펴봤다.</p>
<pre><code class="language-java">public ResponseCookie generateResponseCookie(String token, Integer maxAge) {
        return ResponseCookie.from(&quot;refreshToken&quot;, token)
                .httpOnly(true)
                .secure(true) // https 사용시
                .path(&quot;/&quot;)
                .maxAge(maxAge)
                .sameSite(&quot;None&quot;)
                .build();
    }</code></pre>
<p>이전에 쿠키 설정할 때 다루었던 부분인데 실제로 배포를 하니 피부로 배우는 느낌이었다.</p>
<p>사실 레퍼런스가 부족해서 왜 로컬에선 리프레쉬 토큰을 담은 쿠키가 브라우저 검사 탭에서는 뜨는데 실제로는 저장이 안되었는 지는 잘 모르겠지만 </p>
<p>저기 secure 설정과 관련이 있었던 것 같다. secure 설정을 true로 해주면 쿠키가 브라우저에 전달되어 저장까지 잘 되고, true로 하려면 https 배포를 진행했어야하는 것. 덕분에 잘 배포할 수 있었다.</p>