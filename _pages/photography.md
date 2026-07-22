---
layout: page
title: photography
permalink: /photography/
description: Some pictures I've taken.
nav: false
photos:
  - 1.jpg
  - 2.jpg
  - 3.jpg
  - 4.jpg
  - 5.jpg
  - 6.jpg
  - 7.png
---

<!-- pages/photography.md -->

<style>
  .photo-viewer-wrap {
    display: flex;
    justify-content: center;
  }
  .photo-viewer {
    position: relative;
    user-select: none;
  }
  .photo-viewer img {
    display: block;
    max-width: min(700px, 90vw);
    max-height: min(75vh, 1000px);
    width: auto;
    height: auto;
    border-radius: 6px;
  }
  .photo-nav {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 20%;
    display: flex;
    align-items: center;
    cursor: pointer;
    background: transparent;
    border: none;
    color: transparent;
    transition: background 0.15s ease, color 0.15s ease;
  }
  .photo-nav:hover {
    color: rgba(255, 255, 255, 0.9);
  }
  .photo-nav.prev {
    left: 0;
    justify-content: flex-start;
    border-radius: 6px 0 0 6px;
  }
  .photo-nav.next {
    right: 0;
    justify-content: flex-end;
    border-radius: 0 6px 6px 0;
  }
  .photo-nav span {
    font-size: 2.5rem;
    padding: 0 0.75rem;
    text-shadow: 0 0 6px rgba(0, 0, 0, 0.6);
  }
  .photo-nav.zoom {
    left: 20%;
    width: 60%;
    cursor: zoom-in;
    justify-content: center;
  }
  .photo-zoom-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.85);
    align-items: center;
    justify-content: center;
    z-index: 2000;
    cursor: zoom-out;
  }
  .photo-zoom-overlay.open {
    display: flex;
  }
  .photo-zoom-overlay img {
    max-width: 95vw;
    max-height: 95vh;
    border-radius: 6px;
  }
  .photo-thumbs {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 1rem;
    overflow-x: auto;
    padding-bottom: 0.25rem;
  }
  .photo-thumbs img {
    width: 90px;
    height: 65px;
    object-fit: cover;
    border-radius: 4px;
    cursor: pointer;
    opacity: 0.55;
    flex-shrink: 0;
  }
  .photo-thumbs img.active {
    opacity: 1;
    outline: 2px solid var(--global-theme-color, #2698ba);
    outline-offset: 2px;
  }
</style>

<div class="photo-viewer-wrap">
  <div class="photo-viewer">
    <img id="photo-main" src="{{ '/assets/img/photography/' | append: page.photos.first | relative_url }}" alt="photo 1">
    <button class="photo-nav prev" aria-label="previous photo" title="Previous photo"><span>&#10094;</span></button>
    <button class="photo-nav zoom" aria-label="zoom in" title="Zoom in"></button>
    <button class="photo-nav next" aria-label="next photo" title="Next photo"><span>&#10095;</span></button>
  </div>
</div>

<div id="photo-zoom-overlay" class="photo-zoom-overlay">
  <img id="photo-zoom-img" alt="">
</div>

<div class="photo-thumbs">
  {% for photo in page.photos %}
    <img
      src="{{ '/assets/img/photography/' | append: photo | relative_url }}"
      data-index="{{ forloop.index0 }}"
      class="{% if forloop.first %}active{% endif %}"
      alt="thumbnail {{ forloop.index }}"
    >
  {% endfor %}
</div>

<script>
  (function () {
    var photos = [
      {% for photo in page.photos %}
        "{{ '/assets/img/photography/' | append: photo | relative_url }}"{% unless forloop.last %},{% endunless %}
      {% endfor %}
    ];
    var current = 0;
    var mainImg = document.getElementById('photo-main');
    var thumbs = document.querySelectorAll('.photo-thumbs img');

    function show(index) {
      current = (index + photos.length) % photos.length;
      mainImg.src = photos[current];
      mainImg.alt = 'photo ' + (current + 1);
      thumbs.forEach(function (thumb, i) {
        thumb.classList.toggle('active', i === current);
      });
    }

    document.querySelector('.photo-nav.prev').addEventListener('click', function () {
      show(current - 1);
    });
    document.querySelector('.photo-nav.next').addEventListener('click', function () {
      show(current + 1);
    });
    thumbs.forEach(function (thumb, i) {
      thumb.addEventListener('click', function () {
        show(i);
      });
    });

    var overlay = document.getElementById('photo-zoom-overlay');
    var zoomImg = document.getElementById('photo-zoom-img');

    function openZoom() {
      zoomImg.src = photos[current];
      zoomImg.alt = mainImg.alt;
      overlay.classList.add('open');
    }
    function closeZoom() {
      overlay.classList.remove('open');
    }

    document.querySelector('.photo-nav.zoom').addEventListener('click', openZoom);
    overlay.addEventListener('click', closeZoom);

    document.addEventListener('keydown', function (e) {
      if (e.key === 'ArrowLeft') show(current - 1);
      if (e.key === 'ArrowRight') show(current + 1);
      if (e.key === 'Escape') closeZoom();
    });
  })();
</script>

<!--
Add photos by dropping files into assets/img/photography/ and adding the
filename to the `photos:` list in this page's front matter (order = slideshow
order). The main image is capped to a 450px-tall viewer (object-fit: contain,
so nothing gets cropped); thumbnails are fixed 90x65px tiles. Click the left/
right edges of the main image, the nav arrows, or a thumbnail to change photos;
arrow keys work too.
-->
