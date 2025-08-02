このページでは、AviUtl2で利用できるスクリプト/プラグインを一覧で紹介しています。
AviUtl2で動くスクリプト/プラグインを発見・自作された方は、ぜひ[投稿フォーム](./script_post.md)から投稿してください。


!!! info "スクリプトの導入方法"
    スクリプトの導入方法がわからない場合は、[スクリプトの導入方法](./how_to_install_script.md)のページを参照してください。

!!! Warning "注意"
    このページに掲載されているものは基本管理人が目を通していますが、稀に不適切な内容が含まれる場合があります。
    その場合は、Twitterの[@halkun19](https://twitter.com/halkun19)までご連絡ください。
---

<div id="script-list-container">
  <p>一覧を読み込んでいます...</p>
</div>

<script>
  // ▼▼▼ 下の行のURLを、ステップ1でコピーしたご自身のウェブアプリURLに書き換えてください ▼▼▼
  const GAS_URL = 'https://script.google.com/macros/s/AKfycbykN3EGL9VatVqYz0JphxScas47jj-2we1tUWIyraOpJQIWrX-M77sQIW6dDT7fKETm/exec';
  // ▲▲▲ 上の行のURLを、ステップ1でコピーしたご自身のウェブアプリURLに書き換えてください ▲▲▲

  document.addEventListener('DOMContentLoaded', () => {
    const container = document.getElementById('script-list-container');

    fetch(GAS_URL)
      .then(response => {
        if (!response.ok) throw new Error('Network response was not ok.');
        return response.json();
      })
      .then(data => {
        if (data.scripts && data.scripts.length > 0) {
          let tableHtml = `
            <table>
              <thead>
                <tr><th>種別</th><th>スクリプト名</th><th>説明</th><th>制作者</th><th>ダウンロード</th></tr>
              </thead>
              <tbody>`;

          data.scripts.reverse().forEach(script => { // 新しい投稿が上に来るように逆順にする
            const author = script.author_url ? `<a href="${script.author_url}" target="_blank" rel="noopener noreferrer">${script.author}</a>` : script.author;
            const description = script.description.replace(/\n/g, '<br>');
            tableHtml += `
              <tr>
                <td>${script.script_or_plugin || ''}</td>
                <td>${script.name || ''}</td>
                <td>${description || ''}</td>
                <td>${author || ''}</td>
                <td><a href="${script.download_url}" target="_blank" rel="noopener noreferrer">ダウンロード</a></td>
              </tr>`;
          });

          tableHtml += `</tbody></table>`;
          container.innerHTML = tableHtml;
        } else {
          container.innerHTML = '<p>現在、登録されているスクリプトはありません。</p>';
        }
      })
      .catch(error => {
        console.error('Error fetching script list:', error);
        container.innerHTML = '<p style="color: red;">スクリプト一覧の読み込みに失敗しました。ページを再読み込みするか、管理者にお問い合わせください。</p>';
      });
  });
</script>
