# Url-shortener
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Innovative URL Shortener</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');
  body {
    margin: 0;
    background: linear-gradient(135deg, #667eea, #764ba2);
    font-family: 'Inter', sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 3rem 1rem 6rem;
    color: #fff;
  }
  h1 {
    font-weight: 700;
    font-size: 3rem;
    margin-bottom: 0.2rem;
    text-shadow: 0 2px 6px rgba(0,0,0,0.3);
    user-select:none;
  }
  p.subtitle {
    margin: 0 0 2.5rem 0;
    font-weight: 500;
    font-size: 1.3rem;
    letter-spacing: 0.02em;
    opacity: 0.85;
    user-select:none;
  }
  form#shortenForm {
    width: 100%;
    max-width: 600px;
    display: flex;
    gap: 1rem;
  }
  input#longUrl {
    flex: 1;
    padding: 1rem 1.2rem;
    font-size: 1.1rem;
    border-radius: 10px;
    border: none;
    outline: none;
    box-shadow: inset 2px 2px 8px #5d68c7,
                inset -2px -2px 8px #8a92e7;
    transition: box-shadow 0.25s ease;
    color: #222;
  }
  input#longUrl::placeholder {
    color: #999;
  }
  input#longUrl:focus {
    box-shadow: 0 0 10px 3px #95a0ff;
  }
  button#shortenBtn {
    background: #ffd369;
    color: #222;
    font-weight: 700;
    font-size: 1.15rem;
    border: none;
    border-radius: 10px;
    padding: 0 2rem;
    cursor: pointer;
    box-shadow: 0 6px 14px #e5b64a;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    user-select:none;
  }
  button#shortenBtn:hover {
    background: #ffcb3e;
    box-shadow: 0 8px 20px #f4d56e;
  }

  #errorMsg {
    margin-top: 0.8rem;
    color: #ff4e42;
    font-weight: 600;
    font-size: 1rem;
    min-height: 1.2rem;
    text-align: center;
    user-select:none;
  }

  /* Shortened Links List */
  #linksContainer {
    max-width: 600px;
    width: 100%;
    margin-top: 3rem;
    background: rgba(255 255 255 / 0.12);
    border-radius: 14px;
    padding: 1rem 1.5rem 1.8rem;
    box-shadow: 0 8px 25px rgba(0,0,0,0.35);
    user-select:none;
  }
  #linksContainer h2 {
    margin-top: 0;
    font-weight: 700;
    font-size: 1.8rem;
    margin-bottom: 1rem;
    text-align:center;
    color: #ffd369;
    text-shadow: 1px 1px 3px rgba(0,0,0,0.5);
  }
  #linksList {
    list-style: none;
    margin: 0; padding: 0;
    max-height: 320px;
    overflow-y: auto;
  }
  #linksList li {
    background: rgba(255 255 255 / 0.2);
    border-radius: 12px;
    margin-bottom: 0.8rem;
    padding: 1rem 1.2rem;
    display: flex;
    flex-direction: column;
    gap: 6px;
    box-shadow: inset 2px 2px 5px #5d68c7,
                inset -2px -2px 5px #8a92e7;
  }
  #linksList li:last-child {
    margin-bottom: 0;
  }
  .link-item-original {
    word-break: break-all;
    color: #fff;
    font-weight: 600;
  }
  .link-item-short {
    color: #ffd369;
    font-weight: 700;
    font-size: 1.1rem;
  }
  .link-item-short a {
    color: #ffd369;
    text-decoration: none;
    cursor: pointer;
  }
  .link-item-short a:hover,
  .copy-btn:hover {
    text-decoration: underline;
  }
  .copy-btn {
    background: none;
    border: none;
    color: #ffd369;
    font-weight: 700;
    cursor: pointer;
    align-self: flex-start;
    padding: 0;
    font-size: 0.95rem;
    transition: color 0.25s ease;
  }
  .copy-btn:active {
    color: #ffe87f;
  }
  .stats {
    font-size: 0.9rem;
    font-weight: 600;
    color: #ccc;
    user-select:none;
  }
  #clearBtn {
    margin: 1rem auto 0;
    background: #ff4e42;
    padding: 0.8rem 2rem;
    border-radius: 14px;
    font-weight: 700;
    border: none;
    color: white;
    cursor: pointer;
    display: block;
    box-shadow: 0 4px 12px rgba(255, 78, 66, 0.6);
    transition: background 0.3s ease;
  }
  #clearBtn:hover {
    background: #ff2b1a;
  }

  /* Improved Scrollbar for links list */
  #linksList::-webkit-scrollbar {
    width: 8px;
  }
  #linksList::-webkit-scrollbar-track {
    background: rgba(255 255 255 / 0.1);
    border-radius: 12px;
  }
  #linksList::-webkit-scrollbar-thumb {
    background: #ffd369;
    border-radius: 12px;
  }
  /* Responsive */
  @media (max-width: 640px) {
    form#shortenForm {
      flex-direction: column;
    }
    button#shortenBtn {
      width: 100%;
      padding: 1rem 0;
      border-radius: 10px;
    }
  }
</style>
</head>
<body>
  <h1>URL Shortener</h1>
  <p class="subtitle">Convert long web addresses into short, shareable links with instant analytics!</p>
  <form id="shortenForm" aria-label="URL shortener form">
    <input id="longUrl" type="url" placeholder="Enter or paste a long URL here" aria-required="true" aria-describedby="errorMsg" autocomplete="off" />
    <button id="shortenBtn" type="submit">Shorten</button>
  </form>
  <div id="errorMsg" role="alert" aria-live="assertive"></div>

  <section id="linksContainer" aria-live="polite" aria-atomic="true" aria-label="List of shortened URLs">
    <h2>Your shortened URLs</h2>
    <ul id="linksList" tabindex="0"></ul>
    <button id="clearBtn" aria-label="Clear all shortened URLs">Clear All</button>
  </section>

<script>
  (() => {
    const form = document.getElementById('shortenForm');
    const input = document.getElementById('longUrl');
    const errorMsg = document.getElementById('errorMsg');
    const linksList = document.getElementById('linksList');
    const clearBtn = document.getElementById('clearBtn');

    // Base shorter domain (simulate)
    const BASE_URL = window.location.origin + window.location.pathname.replace(/[^\/]*$/, ''); // current directory

    // Get stored links or empty array
    let storedLinks = JSON.parse(localStorage.getItem('shortenedUrls')) || [];

    function isValidUrl(url) {
      try {
        const parsed = new URL(url);
        return ['http:', 'https:'].includes(parsed.protocol);
      } catch {
        return false;
      }
    }

    // Generate random 6-character short code
    function generateCode() {
      const chars = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
      let code = '';
      for(let i=0; i<6; i++) {
        code += chars.charAt(Math.floor(Math.random() * chars.length));
      }
      return code;
    }

    // Check if code already exists to avoid collisions
    function uniqueCode() {
      let code;
      do {
        code = generateCode();
      } while (storedLinks.find(l => l.code === code));
      return code;
    }

    // Render the list of shortened URLs with stats
    function renderLinks() {
      linksList.innerHTML = '';
      if(storedLinks.length === 0) {
        linksList.innerHTML = '<li style="color:#ccc;text-align:center;">No shortened URLs yet.</li>';
        clearBtn.style.display = 'none';
        return;
      }
      clearBtn.style.display = 'block';

      storedLinks.forEach(({original, code, clicks}) => {
        const shortUrl = BASE_URL + '#' + code;

        const li = document.createElement('li');
        li.tabIndex = 0;

        const originalDiv = document.createElement('div');
        originalDiv.className = 'link-item-original';
        originalDiv.textContent = original;

        const shortDiv = document.createElement('div');
        shortDiv.className = 'link-item-short';

        const a = document.createElement('a');
        a.href = shortUrl;
        a.target = '_blank';
        a.rel = 'noopener noreferrer';
        a.title = 'Open shortened link';
        a.textContent = shortUrl;

        const copyBtn = document.createElement('button');
        copyBtn.className = 'copy-btn';
        copyBtn.type = 'button';
        copyBtn.title = 'Copy shortened URL';
        copyBtn.textContent = 'Copy';

        copyBtn.addEventListener('click', () => {
          navigator.clipboard.writeText(shortUrl).then(() => {
            copyBtn.textContent = 'Copied!';
            setTimeout(() => copyBtn.textContent = 'Copy', 1500);
          });
        });

        shortDiv.appendChild(a);
        shortDiv.appendChild(copyBtn);

        const statsDiv = document.createElement('div');
        statsDiv.className = 'stats';
        statsDiv.textContent = Clicks: ${clicks};

        li.appendChild(originalDiv);
        li.appendChild(shortDiv);
        li.appendChild(statsDiv);

        linksList.appendChild(li);
      });
    }

    // On form submit to shorten URL
    form.addEventListener('submit', e => {
      e.preventDefault();
      errorMsg.textContent = '';
      const longUrl = input.value.trim();

      if(!isValidUrl(longUrl)) {
        errorMsg.textContent = 'Please enter a valid URL starting with http:// or https://';
        return;
      }

      // Check if URL already shortened before
      let foundLink = storedLinks.find(l => l.original === longUrl);
      if(foundLink) {
        // Show message, set input to empty to encourage new links
        errorMsg.style.color = '#ffd369';
        errorMsg.textContent = 'This URL was already shortened.';
        renderLinks();
        input.value = '';
        return;
      }

      const code = uniqueCode();
      const newLink = {
        original: longUrl,
        code: code,
        clicks: 0
      };
      storedLinks.unshift(newLink);
      localStorage.setItem('shortenedUrls', JSON.stringify(storedLinks));
      renderLinks();
      input.value = '';
      errorMsg.style.color = '#00e676';
      errorMsg.textContent = 'URL shortened successfully! Click your shortened link below.';
    });

    // Clear all stored data
    clearBtn.addEventListener('click', () => {
      if(confirm('Are you sure you want to clear all shortened URLs and stats? This action cannot be undone.')) {
        storedLinks = [];
        localStorage.removeItem('shortenedUrls');
        renderLinks();
        errorMsg.textContent = '';
      }
    });

    // Handle redirect functionality - on page load check hash
    function handleRedirect() {
      const hash = window.location.hash.slice(1);
      if(!hash) return;

      // Find original URL by code
      const link = storedLinks.find(l => l.code === hash);
      if(!link) return;

      // Increment clicks and save
      link.clicks++;
      localStorage.setItem('shortenedUrls', JSON.stringify(storedLinks));

      // Redirect to original URL after brief notice
      document.body.innerHTML = `
      <div style="color:#fff; font-family: 'Inter', sans-serif; padding: 3rem; text-align:center; background: #2e2e72; min-height: 100vh;">
        <h1 style="font-weight:700; font-size:2.5rem;">Redirecting...</h1>
        <p style="font-size:1.2rem; margin-top:1.5rem;">You will be redirected to:<br><a href="${link.original}" style="color:#ffd369; text-decoration:underline;" target="_blank" rel="noopener noreferrer">${link.original}</a></p>
        <p style="font-size:1rem; margin-top:2rem;">If the redirection does not happen automatically, click the link above.</p>
      </div>`;
      setTimeout(() => {
        window.location.href = link.original;
      }, 2500);
    }

    // Initial rendering
    renderLinks();
    handleRedirect();

  })();
</script>
</body>
</html>
