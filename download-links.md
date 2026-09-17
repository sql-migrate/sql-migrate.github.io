---
title: "Download PostgresCompare for Windows, Mac and Linux"
description: "Download PostgresCompare and start comparing PostgreSQL schemas in minutes. 14 days of Pro, then the free tier — no expiry, no credit card. Works with PostgreSQL 9.2 through 18."
permalink: /get
sitemap: false
robots: noindex
---

{%- comment -%}
Download URLs: <base>/<version>/<product>-<version>-<os>-<arch>.<ext>, so the
files are immutable and cacheable and a download names its own version. Arch is
normalised to x64 / arm64 — the publish job renames tauri's output, which spells
it three ways. Release day is one app_version edit in _config.yml.

The artifacts for this version are not uploaded yet; delete the warning banner
below once they are.
{%- endcomment -%}
{% assign dl = "https://downloads.postgrescompare.com/builds" %}
{% assign v = site.app_version %}

<section class="page-hero">
  <p class="hunk">@@ <b>download</b> @@</p>
  <h1>Download <span>PostgresCompare</span></h1>
  <p>Fourteen days of Pro, then it becomes the free tier. No expiry, no credit card, no commitments.</p>
</section>

<section class="downloads-section" style="padding: 3rem 2rem; max-width: 1280px; margin: 0 auto;">
  <div class="whatsnew">
    <div class="bar">postgrescompare {{ site.app_version }}</div>
    <p>Pipelines with a visual diagram, official Linux builds, a multi-language interface, live progress on every comparison and deployment, and more reliable dependency ordering in generated scripts. <a href="/release-notes">View all release notes &rarr;</a></p>
  </div>

  <div class="download-cards" id="single-platform-downloads" style="display: none;"></div>

  <p id="show-all-platforms" style="text-align: center; margin-top: 1rem; display: none;">
    <a href="#" onclick="showAllPlatforms(); return false;" style="color: var(--blue);">Show all platforms</a>
  </p>

  <div id="all-platforms-view">
  <div class="download-cards" id="gui-downloads">
    <div class="download-card" data-os="windows">
      <span class="platform-tag">installer</span>
      <h2>Windows</h2>
      <p class="version-info">Version {{ site.app_version }} &bull; Windows 10 or later, 64-bit</p>
      <p style="color: var(--ink-soft); margin-bottom: 1rem;">Signed installer. Installs the desktop app and puts the <code>pgc</code> CLI on your PATH.</p>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-windows-x64-setup.exe" class="download-btn">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
          <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        </svg>
        Download installer
      </a>
    </div>

    <div class="download-card" data-os="macos">
      <span class="platform-tag">.dmg &middot; .pkg</span>
      <h2>macOS</h2>
      <p class="version-info">Version {{ site.app_version }} &bull; macOS 11 or later</p>
      <p style="color: var(--ink-soft); margin-bottom: 1rem;">Signed and notarized, with native builds for Apple Silicon and Intel. Take the <code>.dmg</code> to drag the app across, or the <code>.pkg</code> to run an installer.</p>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-macos-arm64.dmg" class="download-btn">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
          <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        </svg>
        Apple Silicon (.dmg)
      </a>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-macos-arm64.pkg" class="download-alt">Apple Silicon installer (.pkg) &rarr;</a>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-macos-x64.dmg" class="download-alt">Intel (.dmg) &rarr;</a>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-macos-x64.pkg" class="download-alt">Intel installer (.pkg) &rarr;</a>
    </div>

    <div class="download-card" data-os="linux">
      <span class="platform-tag">.AppImage &middot; .deb</span>
      <h2>Linux</h2>
      <p class="version-info">Version {{ site.app_version }} &bull; 64-bit, Ubuntu 20.04+ / Debian 11+</p>
      <p style="color: var(--ink-soft); margin-bottom: 1rem;">The AppImage runs anywhere without installing. The <code>.deb</code> installs it properly on Debian and Ubuntu.</p>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-linux-x64.AppImage" class="download-btn">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
          <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        </svg>
        AppImage
      </a>
      <a href="{{ dl }}/{{ v }}/PostgresCompare-{{ v }}-linux-x64.deb" class="download-alt">Debian / Ubuntu package (.deb) &rarr;</a>
    </div>
  </div>

  <div style="margin-top: 4rem;">
    <p class="hunk">@@ <b>command line interface</b> @@</p>
    <h2 class="d-h2">Command line interface</h2>
    <p class="d-intro">For CI/CD pipelines and automation workflows. The <code>pgc</code> CLI is part of Pro, and the 14-day trial includes it.</p>
  </div>

  <div class="download-cards" id="cli-downloads">
    <div class="download-card" data-os="windows">
      <span class="platform-tag">pgc.exe</span>
      <h3 class="cli-platform">Windows CLI</h3>
      <p class="version-info">Version {{ site.app_version }} &bull; x64</p>
      <a href="{{ dl }}/{{ v }}/pgc-{{ v }}-windows-x64.zip" class="download-btn">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
          <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        </svg>
        Download CLI
      </a>
    </div>

    <div class="download-card" data-os="macos">
      <span class="platform-tag">pgc</span>
      <h3 class="cli-platform">macOS CLI</h3>
      <p class="version-info">Version {{ site.app_version }} &bull; Apple Silicon and Intel</p>
      <a href="{{ dl }}/{{ v }}/pgc-{{ v }}-macos-arm64.tar.gz" class="download-btn">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
          <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        </svg>
        Apple Silicon
      </a>
      <a href="{{ dl }}/{{ v }}/pgc-{{ v }}-macos-x64.tar.gz" class="download-alt">Intel Mac (x64) &rarr;</a>
    </div>

    <div class="download-card" data-os="linux">
      <span class="platform-tag">pgc</span>
      <h3 class="cli-platform">Linux CLI</h3>
      <p class="version-info">Version {{ site.app_version }} &bull; x64</p>
      <a href="{{ dl }}/{{ v }}/pgc-{{ v }}-linux-x64.tar.gz" class="download-btn">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
          <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        </svg>
        Tarball
      </a>
      <a href="{{ dl }}/{{ v }}/pgc-{{ v }}-linux-x64.deb" class="download-alt">Debian / Ubuntu package (.deb) &rarr;</a>
    </div>
  </div>
  </div>

  <div class="trial-info">
    <h2 class="d-h2">14 days of Pro, then free forever</h2>
    <p class="d-intro">Every download starts with 14 days of Pro. When the trial ends the app becomes the free tier — Quick compare, deployment scripts and deploying all keep working. Saved projects, history, pipelines and the CLI are what a subscription adds. No credit card, and nothing expires into a locked window.</p>
    <div class="trial-features">
      <div class="trial-feature">
        <i class="ok">✓</i>
        <span><strong>14 days of Pro</strong></span>
      </div>
      <div class="trial-feature">
        <i class="ok">✓</i>
        <span><strong>Then free, forever</strong></span>
      </div>
      <div class="trial-feature">
        <i class="ok">✓</i>
        <span><strong>No expiry cliff</strong></span>
      </div>
      <div class="trial-feature">
        <i class="ok">✓</i>
        <span><strong>No credit card</strong></span>
      </div>
    </div>
  </div>

  <div style="margin-bottom: 4rem;">
    <p class="hunk">@@ <b>system requirements</b> @@</p>
    <h2 class="d-h2">System requirements</h2>

    <div class="d-grid">
      <div class="cell">
        <h3>PostgreSQL version</h3>
        <p>Supports PostgreSQL 9.2 through 18</p>
      </div>
      <div class="cell">
        <h3>Disk space</h3>
        <p>250 MB free disk space for installation</p>
      </div>
      <div class="cell">
        <h3>Memory</h3>
        <p>4 GB RAM minimum, 8 GB recommended</p>
      </div>
      <div class="cell">
        <h3>Network access</h3>
        <p>Direct connection to PostgreSQL databases on your network</p>
      </div>
    </div>
  </div>
</section>

<script>
(function() {
  function detectOS() {
    var ua = navigator.userAgent.toLowerCase();
    if (ua.indexOf('win') !== -1) return 'windows';
    if (ua.indexOf('mac') !== -1) return 'macos';
    if (ua.indexOf('linux') !== -1) return 'linux';
    return null;
  }

  // Shows just the visitor's platform. Each OS is a single card holding every
  // option for it, so one card per section is still the right thing to clone.
  function showSinglePlatform(os) {
    var singleView = document.getElementById('single-platform-downloads');
    var allView = document.getElementById('all-platforms-view');
    var showAllLink = document.getElementById('show-all-platforms');

    var guiCard = document.querySelector('#gui-downloads .download-card[data-os="' + os + '"]');
    var cliCard = document.querySelector('#cli-downloads .download-card[data-os="' + os + '"]');

    if (guiCard && cliCard) {
      singleView.innerHTML = '';
      var guiClone = guiCard.cloneNode(true);
      var cliClone = cliCard.cloneNode(true);

      var guiHeading = guiClone.querySelector('h2');
      if (guiHeading) guiHeading.innerHTML = 'Desktop app';
      var guiIcon = guiClone.querySelector('.platform-tag');
      if (guiIcon) guiIcon.textContent = 'desktop';

      var cliHeading = cliClone.querySelector('h3');
      if (cliHeading) cliHeading.outerHTML = '<h2>Command line</h2>';
      var cliIcon = cliClone.querySelector('.platform-tag');
      if (cliIcon) cliIcon.textContent = 'cli';
      var cliVersionInfo = cliClone.querySelector('.version-info');
      if (cliVersionInfo) cliVersionInfo.insertAdjacentHTML('afterend', '<p style="color: var(--ink-soft); margin-bottom: 1rem;">For CI/CD pipelines and automation workflows.</p>');

      singleView.appendChild(guiClone);
      singleView.appendChild(cliClone);

      singleView.style.display = '';
      allView.style.display = 'none';
      if (showAllLink) showAllLink.style.display = 'block';
    }
  }

  window.showAllPlatforms = function() {
    var singleView = document.getElementById('single-platform-downloads');
    var allView = document.getElementById('all-platforms-view');
    var showAllLink = document.getElementById('show-all-platforms');

    singleView.style.display = 'none';
    allView.style.display = '';
    if (showAllLink) showAllLink.style.display = 'none';
  };

  document.addEventListener('DOMContentLoaded', function() {
    var os = detectOS();
    if (os) showSinglePlatform(os);
  });
})();
</script>
