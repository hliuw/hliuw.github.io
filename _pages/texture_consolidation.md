---
layout: page
title: Texture Consolidation
permalink: /texture-consolidation/
description: Project page for "Texture Consolidation of Generated Multi-View Renderings"
nav: false

# Result viewer data. Each object needs one rendering per method at
# assets/img/texture_consolidation/<object>_<method>.webp, all rendered from
# the same camera at the same framing so that a point maps to the same pixel
# in every method. `regions` are the areas the paper's figure zooms into, as
# (fraction of width, fraction of height) centers, in the paper's order.
#
# Source: the Fig. 8 renders in paper_figure/baselilne_comparison_new/<id>/
# (Mask 78df, Penguin 5487, Pharaoh 3c48), 1024x1024, each object cropped to
# one square shared by all of its methods. Verified against the paper figure.
tc_objects:
  - key: mask
    name: Mask
    regions:
      - { name: Swirl, at: [0.251, 0.47] }
      - { name: Ornament, at: [0.344, 0.815] }
  - key: penguin
    name: Penguin
    regions:
      - { name: Flag, at: [0.441, 0.456] }
      - { name: Eye, at: [0.373, 0.126] }
  - key: pharaoh
    name: Pharaoh
    regions:
      - { name: Lips, at: [0.498, 0.49] }
      - { name: Head-piece, at: [0.639, 0.157] }

# Baselines, in the paper's column order. Notes paraphrase Sec. 7.2.
tc_methods:
  - key: blended
    name: Blended
    note: Confidence-weighted blending of the back-projected renderings, with no registration, so misaligned features ghost.
  - key: mvstex
    name: MVS-tex
    note: Takes each patch from a single view. Consistent within a patch, but leaves seams at patch boundaries and raises contrast.
  - key: colormap
    name: ColorMap
    note: Registration designed for dense RGB-D video. It degrades on a sparse set of generated views.
  - key: advia
    name: AdvIA
    note: Adversarial texture optimization designed for dense RGB-D video. It degrades on sparse generated views and shifts colors.
  - key: im2surftex
    name: Im2SurfTex
    note: Aggregates colors over small, fixed pixel-radius neighborhoods, so misalignments larger than that remain as blur and ghosting.
  - key: meshlrm
    name: NeRF (MeshLRM)
    note: View-dependent color reproduces the input views, but shows artifacts at views outside the input set, such as this one.
tc_default_method: blended
---

{% comment %}
  DRAFT project page. The paper is an arXiv preprint (not published at a venue).
  Before making this public:
    1. Fill in the arXiv URL, and the arXiv ID in the BibTeX (XXXX.XXXXX).
    2. Once github.com/hliuw/TextureConsolidation is public, restore the Code
       button and the 3D section's "code release" link (both in Liquid comments).
  The BibTeX section and button are hidden in Liquid comments for now.
  Images in assets/img/texture_consolidation/ are renderings only.
  No mesh or texture data may be added there (copyright).
  The vase mesh and textures in assets/texture_consolidation/vase/ are cleared
  for distribution; keep the ISTI-CNR credit line next to the viewer.
{% endcomment %}

{% assign tc_first = page.tc_objects | first %}
{% assign tc_img = '/assets/img/texture_consolidation/' | relative_url %}

<div class="tc-paper">

  <h1 class="tc-title">Texture Consolidation of Generated Multi-View Renderings</h1>

  <div class="tc-authors">
    <span>Hongyi Liu<sup>1</sup></span>
    <span>Yifan Wang<sup>2</sup></span>
    <span>Vladimir G. Kim<sup>3</sup></span>
    <span>Misha Kazhdan<sup>1</sup></span>
  </div>

  <div class="tc-affil">
    <span><sup>1</sup>Johns Hopkins University</span>
    <span><sup>2</sup>Adobe Research</span>
    <span><sup>3</sup>Adobe Firefly Foundry</span>
  </div>

  <nav class="tc-links" aria-label="Paper resources">
    <a href="#" class="tc-link" aria-disabled="true">arXiv</a>
    {% comment %} Code is hidden until github.com/hliuw/TextureConsolidation is public. {% endcomment %}
    {% comment %}<a href="https://github.com/hliuw/TextureConsolidation" class="tc-link">Code</a>{% endcomment %}
    {% comment %} BibTeX is hidden for now; restore together with the section below. {% endcomment %}
    {% comment %}<a href="#bibtex" class="tc-link">BibTeX</a>{% endcomment %}
  </nav>

  <figure class="tc-teaser">
    <img
      src="{{ tc_img }}teaser.webp"
      alt="Pipeline overview: multi-view renderings are back-projected into texture-space colors and
           confidences, registered by a flow-field on the surface, and fused by gradient-domain
           stitching; naive blending ghosts where the registered-and-stitched result stays sharp."
      width="2400" height="768" decoding="async">
    <figcaption>
      <p>
        <span class="tc-io">Input</span>
        A sparse set of generated multi-view renderings with known cameras (left), together with a
        texture-atlased mesh.
      </p>
      <p>
        We back-project the renderings into texture-space colors and confidences, register them with
        flow-fields solved on the surface, and fuse them by confidence-aware gradient-domain stitching.
      </p>
      <p>
        <span class="tc-io">Output</span>
        A single seamless texture image, shown rendered on the mesh (right). Confidence-weighted
        blending alone ghosts; registration and stitching keep the detail sharp.
      </p>
    </figcaption>
  </figure>

  <h2>Abstract</h2>

  <p class="tc-abstract">
    We present a system for consolidating a sparse set of multi-view renderings into a single
    coherent texture image, providing a framework for integrating generative models for image
    generation within the geometry-processing pipeline. Given a texture-atlased triangle mesh and a
    set of renderings with associated camera parameters, we generate the texture image by:
    (1) back-projecting the renderings onto the mesh to obtain texture images with normalized colors
    and confidences; (2) solving for flow-fields on the mesh that align the colors to each other,
    taking into account the associated confidences; and (3) fusing the registered colors into a
    single texture image using gradient-domain stitching, again incorporating confidences. Our
    approach complements existing techniques for multi-view image generation, leveraging mesh-based
    optical flow and texture-space stitching. In doing so it addresses the challenge that while
    modern image generation pipelines generate highly convincing renderings, they struggle to
    produce renderings that are consistent across views.
  </p>

  <h2 id="real-photographs">Results on Real Photographs</h2>

  <p class="tc-caption-lead">
    {% comment %} Link "code release" to github.com/hliuw/TextureConsolidation once it is public. {% endcomment %}
    Here we show the example run from our code release, with nine photographs of a vase as input.
  </p>

  <p class="tc-howto">
    Drag to rotate, scroll or pinch to zoom, and drag the divider to compare <strong>Blended</strong>
    on the left with <strong>Ours</strong> on the right.
  </p>

  <figure class="tc-3d" id="tc-3d"
          data-base="{{ '/assets/texture_consolidation/vase/' | relative_url }}">
    <div class="tc-3d-stage">
      <canvas class="tc-3d-canvas" aria-label="3D view of the textured vase. Drag to rotate, scroll to zoom."></canvas>
      <div class="tc-3d-divider" role="slider" tabindex="0" aria-label="Comparison divider: Blended on the left, Ours on the right"
           aria-valuemin="0" aria-valuemax="100" aria-valuenow="50" hidden>
        <span class="tc-3d-handle" aria-hidden="true"></span>
      </div>
      <span class="tc-tag tc-3d-tag-left" hidden>Blended</span>
      <span class="tc-tag tc-3d-tag-right" hidden>Ours</span>
      <button type="button" class="tc-3d-reset" hidden>Reset view</button>
      <span class="tc-3d-tap" aria-hidden="true" hidden>tap to interact</span>
      <p class="tc-3d-status">Loading the 3D model&hellip;</p>
    </div>
    <figcaption class="tc-credit">
      The vase model and photographs used in the example run are courtesy of Paolo Cignoni and Marco
      Callieri, <a href="https://vcg.isti.cnr.it/">Visual Computing Laboratory, ISTI-CNR</a>.
    </figcaption>
  </figure>

  <h2 id="generated-renderings">Results on Generated Renderings</h2>

  <p class="tc-caption-lead">
    The following image shows the front view of three objects, each textured from nine generated
    renderings. Note that the front view is <strong class="tc-hl">not</strong>
    one of the nine inputs.
  </p>

  <p class="tc-howto">
    Use <strong>Highlights</strong> to step through the regions the paper zooms into, or
    <strong>Free zoom</strong> to magnify whatever is under the cursor. The row under the image shows
    every method at the same spot. Click one to place it beside ours.
  </p>

  <figure class="tc-viewer" id="tc-viewer"
          data-base="{{ tc_img }}"
          data-method="{{ page.tc_default_method }}"
          data-mode="regions">

    <script type="application/json" class="tc-data">{{ page.tc_objects | jsonify }}</script>

    <div class="tc-toolbar">
      <div class="tc-seg" role="group" aria-label="Object">
        {% for o in page.tc_objects %}
          <button type="button" data-obj="{{ o.key }}"
                  aria-pressed="{% if forloop.first %}true{% else %}false{% endif %}">{{ o.name }}</button>
        {% endfor %}
      </div>

      <div class="tc-seg" role="group" aria-label="Zoom mode">
        <button type="button" data-mode="regions" aria-pressed="true">Highlights</button>
        <button type="button" data-mode="free" aria-pressed="false">Free zoom</button>
      </div>
    </div>

    <div class="tc-stage">

      <div class="tc-full">
        <div class="tc-frame" tabindex="0"
             aria-label="Rendering. In Free zoom, move the pointer or use the arrow keys to choose the magnified point.">
          <img class="tc-frame-img"
               src="{{ tc_img }}{{ tc_first.key }}_ours.webp"
               alt="{{ tc_first.name }}, front view, textured by our method"
               draggable="false" decoding="async">
          <div class="tc-marks"></div>
          <div class="tc-lens" aria-hidden="true"></div>
          <span class="tc-hint" aria-hidden="true">
            <span class="tc-hint-fine">move to magnify</span><span class="tc-hint-coarse">tap to magnify</span>
          </span>
        </div>

        <div class="tc-seg tc-seg-small" role="group" aria-label="Full view">
          <button type="button" data-full="ours" aria-pressed="true">Ours</button>
          <button type="button" data-full="baseline" aria-pressed="false" class="tc-bl-name"></button>
        </div>
      </div>

      <div class="tc-side">
        <div class="tc-pair">
          <div class="tc-zoom">
            <div class="tc-zoom-img" data-zoomof="baseline" role="img" aria-label="Magnified baseline"></div>
            <span class="tc-tag tc-bl-name"></span>
          </div>
          <div class="tc-zoom tc-zoom-ours">
            <div class="tc-zoom-img" data-zoomof="ours" role="img" aria-label="Magnified result, ours"></div>
            <span class="tc-tag">Ours</span>
          </div>
        </div>

        <p class="tc-note" aria-live="polite"></p>

        <div class="tc-regionbar">
          <span class="tc-label" id="tc-region-label">Region</span>
          <div class="tc-seg" role="group" aria-labelledby="tc-region-label"></div>
        </div>
      </div>

    </div>

    <div class="tc-strip">
      <span class="tc-label" id="tc-strip-label">All methods at this spot &middot; choose one to compare</span>
      <div class="tc-tiles" role="group" aria-labelledby="tc-strip-label">
        {% for m in page.tc_methods %}
          <button type="button" class="tc-tile" data-method="{{ m.key }}" data-note="{{ m.note | escape }}"
                  aria-pressed="{% if m.key == page.tc_default_method %}true{% else %}false{% endif %}">
            <span class="tc-tile-img" data-zoomof="{{ m.key }}"></span>
            <span class="tc-tile-name">{{ m.name }}</span>
          </button>
        {% endfor %}
        <div class="tc-tile tc-tile-ours">
          <span class="tc-tile-img" data-zoomof="ours"></span>
          <span class="tc-tile-name">Ours</span>
        </div>
      </div>
    </div>

  </figure>

  <p class="tc-conclusion">
    Fusing misaligned renderings directly leaves ghosting and blur. Selecting one view per patch, as
    MVS-tex does, stays sharp inside each patch but leaves seams where patches meet. Methods designed
    for dense RGB-D captures, such as ColorMap and AdvIA, degrade on a sparse set of inputs.
    <strong class="tc-hl">Our method registers the renderings on the surface before fusing them,
    producing a seamless, ghost-free texture.</strong>
  </p>

{% comment %}
  BibTeX is hidden for now. To restore it, remove this comment wrapper and the
  one around the BibTeX button in the link row.

  <h2 id="bibtex">BibTeX</h2>

<pre class="tc-bibtex"><code>@article{liu2026texture,
  title   = {Texture Consolidation of Generated Multi-View Renderings},
  author  = {Liu, Hongyi and Wang, Yifan and Kim, Vladimir G. and Kazhdan, Misha},
  journal = {arXiv preprint arXiv:XXXX.XXXXX},
  year    = {2026}
}</code></pre>
{% endcomment %}

</div>

<style>
  /* This page supplies its own centered title block, so suppress the layout's
     header. The front-matter title is kept for the browser tab and SEO. */
  .post-header {
    display: none;
  }

  .tc-paper {
    max-width: 56rem;
    margin: 0 auto;
  }

  /* Generous separation between the page's major sections. */
  .tc-paper h2 {
    margin-top: 4rem;
    margin-bottom: 1rem;
  }

  .tc-title {
    font-size: clamp(1.5rem, 4vw, 2.1rem);
    line-height: 1.25;
    text-align: center;
    margin-bottom: 1.2rem;
  }

  .tc-authors,
  .tc-affil,
  .tc-links {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.4rem 1.4rem;
  }

  .tc-authors {
    font-size: 1.05rem;
  }

  .tc-affil {
    font-size: 0.88rem;
    opacity: 0.75;
    margin-top: 0.35rem;
  }

  .tc-links {
    margin: 1.5rem 0 0.5rem;
    gap: 0.6rem;
  }

  .tc-link {
    display: inline-block;
    padding: 0.4rem 1.1rem;
    border: 1px solid var(--global-divider-color);
    border-radius: 2rem;
    font-size: 0.92rem;
    text-decoration: none;
    color: var(--global-text-color);
    transition: background-color 0.15s ease, border-color 0.15s ease;
  }

  .tc-link:hover {
    background-color: var(--global-theme-color);
    border-color: var(--global-theme-color);
    color: #fff;
    text-decoration: none;
  }

  /* Placeholder links: visibly inert until real URLs are filled in. */
  .tc-link[aria-disabled="true"] {
    opacity: 0.45;
    cursor: not-allowed;
  }

  /* The figures are authored on white, so they keep a white plate in both
     themes rather than floating on a dark page with a hard rectangular edge. */
  .tc-teaser {
    margin: 3rem 0 0;
  }

  .tc-teaser img {
    width: 100%;
    height: auto;
    display: block;
    background-color: #fff;
    border-radius: 0.4rem;
    padding: 0.6rem;
  }

  .tc-teaser figcaption {
    margin-top: 1rem;
    font-size: 0.9rem;
  }

  .tc-teaser figcaption p {
    margin: 0 0 0.45rem;
  }

  .tc-teaser figcaption p:not(:has(.tc-io)) {
    opacity: 0.75;
  }

  /* Input / Output labels in the teaser caption. */
  .tc-io {
    display: inline-block;
    min-width: 4.6rem;
    margin-right: 0.45rem;
    padding: 0.08rem 0.55rem;
    border-radius: 999px;
    background-color: var(--global-theme-color);
    color: #fff;
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-align: center;
    text-transform: uppercase;
    vertical-align: 0.08em;
  }

  .tc-abstract {
    text-align: justify;
  }

  .tc-caption-lead {
    font-size: 0.95rem;
  }

  /* Emphasis for the key words and the takeaway sentence. */
  .tc-hl {
    color: var(--global-theme-color);
  }

  /* How-to lines sit between a section's description and its figure, set
     apart so they read as instructions rather than as claims. */
  .tc-howto {
    margin: 0.2rem 0 0;
    padding-left: 0.8rem;
    border-left: 2px solid var(--global-theme-color);
    font-size: 0.88rem;
    opacity: 0.85;
  }

  .tc-conclusion {
    margin-top: 1.5rem;
    font-size: 0.95rem;
  }

  /* ---- result viewer ---- */

  .tc-viewer {
    margin: 1.5rem 0 0;
  }

  .tc-toolbar {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 1rem;
  }

  .tc-label {
    display: block;
    font-size: 0.75rem;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    opacity: 0.6;
    margin-bottom: 0.45rem;
  }

  /* Segmented control shared by every button group in the viewer. */
  .tc-seg {
    display: inline-flex;
    flex-wrap: wrap;
    gap: 2px;
    padding: 3px;
    border: 1px solid var(--global-divider-color);
    border-radius: 999px;
  }

  .tc-seg button {
    appearance: none;
    border: 0;
    background: transparent;
    color: var(--global-text-color);
    font: inherit;
    font-size: 0.88rem;
    line-height: 1.2;
    padding: 0.35rem 0.95rem;
    border-radius: 999px;
    cursor: pointer;
    transition: background-color 0.15s ease, color 0.15s ease;
  }

  .tc-seg button:hover {
    color: var(--global-theme-color);
  }

  .tc-seg button[aria-pressed="true"] {
    background-color: var(--global-theme-color);
    color: #fff;
  }

  .tc-seg-small {
    display: flex;
    width: max-content;
    margin: 0.6rem auto 0;
  }

  .tc-seg-small button {
    font-size: 0.8rem;
    padding: 0.25rem 0.8rem;
  }

  .tc-seg button:focus-visible,
  .tc-tile:focus-visible,
  .tc-mark:focus-visible,
  .tc-frame:focus-visible {
    outline: 2px solid var(--global-theme-color);
    outline-offset: 2px;
  }

  .tc-stage {
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(0, 1.08fr);
    gap: 1.25rem;
    align-items: start;
  }

  .tc-frame {
    position: relative;
    overflow: hidden;
    background-color: #fff;
    border-radius: 0.5rem;
    user-select: none;
    -webkit-user-select: none;
  }

  .tc-viewer[data-mode="free"] .tc-frame {
    cursor: crosshair;
    /* Vertical swipes still scroll the page on touch screens; a tap places the lens. */
    touch-action: pan-y;
  }

  .tc-frame-img {
    display: block;
    width: 100%;
    height: auto;
    -webkit-user-drag: none;
  }

  /* The outer shadow dims everything outside the lens; overflow on the frame
     clips it to the rendering. */
  .tc-lens {
    position: absolute;
    left: 0;
    top: 0;
    border: 2px solid var(--global-theme-color);
    border-radius: 3px;
    box-shadow:
      inset 0 0 0 1px rgba(255, 255, 255, 0.85),
      0 0 0 100vmax rgba(0, 0, 0, 0.06);
    pointer-events: none;
    will-change: transform;
  }

  /* Highlight regions: the active one is drawn by the lens, the others as
     dashed outlines that can be clicked. Each carries its number. */
  .tc-marks {
    position: absolute;
    inset: 0;
  }

  .tc-viewer[data-mode="free"] .tc-marks {
    display: none;
  }

  .tc-mark {
    position: absolute;
    left: 0;
    top: 0;
    padding: 0;
    border: 2px dashed rgba(0, 0, 0, 0.45);
    border-radius: 3px;
    background: transparent;
    cursor: pointer;
    transition: border-color 0.15s ease;
  }

  .tc-mark:hover {
    border-color: var(--global-theme-color);
  }

  .tc-mark.is-active {
    border-color: transparent;
  }

  /* Inside the corner, so a region clamped against the frame edge never has
     its number clipped. */
  .tc-mark-num {
    position: absolute;
    left: 0.2rem;
    top: 0.2rem;
    width: 1.15rem;
    height: 1.15rem;
    border-radius: 50%;
    background-color: #fff;
    color: #222;
    box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.35);
    font-size: 0.68rem;
    font-weight: 600;
    line-height: 1.15rem;
    text-align: center;
  }

  .tc-mark.is-active .tc-mark-num {
    background-color: var(--global-theme-color);
    color: #fff;
    box-shadow: none;
  }

  .tc-hint {
    display: none;
    position: absolute;
    left: 50%;
    bottom: 0.7rem;
    transform: translateX(-50%);
    padding: 0.2rem 0.7rem;
    border-radius: 999px;
    background-color: rgba(0, 0, 0, 0.6);
    color: #fff;
    font-size: 0.72rem;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    white-space: nowrap;
    pointer-events: none;
    transition: opacity 0.3s ease;
  }

  .tc-viewer[data-mode="free"] .tc-hint {
    display: block;
  }

  .tc-viewer.tc-touched .tc-hint {
    opacity: 0;
  }

  .tc-hint-coarse {
    display: none;
  }

  @media (pointer: coarse) {
    .tc-hint-fine {
      display: none;
    }

    .tc-hint-coarse {
      display: inline;
    }
  }

  .tc-pair {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.6rem;
  }

  .tc-zoom {
    position: relative;
  }

  .tc-zoom-img,
  .tc-tile-img {
    display: block;
    aspect-ratio: 1 / 1;
    width: 100%;
    background-color: #fff;
    background-repeat: no-repeat;
    border: 1px solid var(--global-divider-color);
    border-radius: 0.5rem;
  }

  .tc-zoom-ours .tc-zoom-img {
    border-color: var(--global-theme-color);
    box-shadow: 0 0 0 1px var(--global-theme-color);
  }

  .tc-tag {
    position: absolute;
    left: 0.5rem;
    top: 0.5rem;
    padding: 0.12rem 0.55rem;
    border-radius: 999px;
    background-color: rgba(0, 0, 0, 0.6);
    color: #fff;
    font-size: 0.72rem;
    letter-spacing: 0.02em;
    pointer-events: none;
  }

  .tc-zoom-ours .tc-tag {
    background-color: var(--global-theme-color);
  }

  /* Per-method notes are hidden for now. The text stays in tc_methods and is
     still filled in by the script; delete this display rule to show it again. */
  .tc-note {
    display: none;
    font-size: 0.84rem;
    opacity: 0.75;
    margin: 0.8rem 0 0;
    min-height: 2.6em;
  }

  .tc-regionbar {
    margin-top: 1rem;
  }

  /* Matches the numbered badge on the region's outline in the rendering. */
  .tc-rnum {
    display: inline-block;
    width: 1.1rem;
    height: 1.1rem;
    margin-right: 0.4rem;
    border-radius: 50%;
    box-shadow: 0 0 0 1px currentColor;
    font-size: 0.68rem;
    font-weight: 600;
    line-height: 1.1rem;
    text-align: center;
    vertical-align: 0.06em;
  }

  .tc-viewer[data-mode="free"] .tc-regionbar {
    visibility: hidden;
  }

  /* ---- row of every method ---- */

  .tc-strip {
    margin-top: 1.75rem;
  }

  .tc-tiles {
    display: grid;
    grid-template-columns: repeat(7, minmax(0, 1fr));
    gap: 0.5rem;
  }

  .tc-tile {
    appearance: none;
    display: block;
    padding: 0;
    border: 0;
    background: transparent;
    color: var(--global-text-color);
    font: inherit;
    text-align: center;
    cursor: pointer;
  }

  .tc-tile-ours {
    cursor: default;
  }

  .tc-tile:not(.tc-tile-ours):hover .tc-tile-img {
    border-color: var(--global-theme-color);
  }

  .tc-tile[aria-pressed="true"] .tc-tile-img {
    border-color: var(--global-text-color);
    box-shadow: 0 0 0 1px var(--global-text-color);
  }

  .tc-tile-ours .tc-tile-img {
    border-color: var(--global-theme-color);
    box-shadow: 0 0 0 1px var(--global-theme-color);
  }

  .tc-tile-name {
    display: block;
    margin-top: 0.35rem;
    font-size: 0.76rem;
    line-height: 1.25;
  }

  .tc-tile[aria-pressed="true"] .tc-tile-name {
    font-weight: 600;
  }

  .tc-tile-ours .tc-tile-name {
    color: var(--global-theme-color);
    font-weight: 600;
  }

  /* ---- 3D comparison ---- */

  .tc-3d {
    margin: 1.5rem 0 0;
  }

  .tc-3d-stage {
    position: relative;
    aspect-ratio: 4 / 3;
    overflow: hidden;
    background-color: #fff;
    border: 1px solid var(--global-divider-color);
    border-radius: 0.5rem;
  }

  .tc-3d-canvas {
    position: absolute;
    inset: 0;
    display: block;
    width: 100%;
    height: 100%;
    cursor: grab;
  }

  .tc-3d-canvas:active {
    cursor: grabbing;
  }

  /* The divider's hit area is wider than the visible line, so it is easy to
     grab without stealing drags meant for rotating the model. */
  .tc-3d-divider {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 50%;
    width: 2rem;
    margin-left: -1rem;
    cursor: ew-resize;
    touch-action: none;
    outline: none;
  }

  .tc-3d-divider::before {
    content: "";
    position: absolute;
    top: 0;
    bottom: 0;
    left: 50%;
    width: 2px;
    margin-left: -1px;
    background-color: var(--global-theme-color);
  }

  .tc-3d-handle {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 1.9rem;
    height: 1.9rem;
    margin: -0.95rem 0 0 -0.95rem;
    border: 2px solid var(--global-theme-color);
    border-radius: 50%;
    background-color: #fff;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.25);
  }

  /* Two chevrons, drawn with borders so they need no icon font. */
  .tc-3d-handle::before,
  .tc-3d-handle::after {
    content: "";
    position: absolute;
    top: 50%;
    width: 0.38rem;
    height: 0.38rem;
    margin-top: -0.19rem;
    border: solid var(--global-theme-color);
    border-width: 0 0 2px 2px;
  }

  .tc-3d-handle::before {
    left: 0.42rem;
    transform: rotate(45deg);
  }

  .tc-3d-handle::after {
    right: 0.42rem;
    transform: rotate(-135deg);
  }

  .tc-3d-divider:focus-visible .tc-3d-handle {
    outline: 2px solid var(--global-theme-color);
    outline-offset: 2px;
  }

  .tc-3d-tag-right {
    left: auto;
    right: 0.5rem;
    background-color: var(--global-theme-color);
  }

  .tc-3d-reset {
    position: absolute;
    right: 0.5rem;
    bottom: 0.5rem;
    appearance: none;
    padding: 0.2rem 0.7rem;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 999px;
    background-color: rgba(255, 255, 255, 0.9);
    color: #222;
    font: inherit;
    font-size: 0.75rem;
    cursor: pointer;
  }

  .tc-3d-reset:hover {
    border-color: var(--global-theme-color);
    color: var(--global-theme-color);
  }

  .tc-3d-tap {
    position: absolute;
    left: 50%;
    bottom: 0.7rem;
    transform: translateX(-50%);
    padding: 0.2rem 0.7rem;
    border-radius: 999px;
    background-color: rgba(0, 0, 0, 0.6);
    color: #fff;
    font-size: 0.72rem;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    pointer-events: none;
  }

  .tc-3d-status {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    color: #666;
    font-size: 0.9rem;
  }

  /* display:flex above would otherwise override the hidden attribute. */
  .tc-3d-status[hidden] {
    display: none;
  }

  .tc-credit {
    margin-top: 0.7rem;
    font-size: 0.8rem;
    opacity: 0.7;
  }

  .tc-bibtex {
    background-color: var(--global-code-bg-color);
    border-radius: 0.4rem;
    padding: 1rem;
    overflow-x: auto;
    font-size: 0.82rem;
  }

  @media (max-width: 720px) {
    .tc-stage {
      grid-template-columns: minmax(0, 1fr);
    }

    .tc-toolbar {
      justify-content: center;
    }

    /* Seven tiles do not fit across a phone; the row scrolls on its own. */
    .tc-tiles {
      display: flex;
      overflow-x: auto;
      scroll-snap-type: x proximity;
      padding-bottom: 0.4rem;
    }

    .tc-tile {
      flex: 0 0 6.2rem;
      scroll-snap-align: start;
    }

    .tc-paper h2 {
      margin-top: 3rem;
    }

    .tc-3d-stage {
      aspect-ratio: 1 / 1;
    }
  }

  @media (prefers-reduced-motion: reduce) {
    .tc-seg button,
    .tc-link,
    .tc-mark,
    .tc-hint {
      transition: none;
    }
  }
</style>

<script>
  (function () {
    var root = document.getElementById('tc-viewer');
    if (!root) {
      return;
    }

    var ZOOM = 2;
    var objects = {};
    JSON.parse(root.querySelector('.tc-data').textContent).forEach(function (o) {
      objects[o.key] = o;
    });

    function all(sel) {
      return Array.prototype.slice.call(root.querySelectorAll(sel));
    }

    var base = root.getAttribute('data-base');
    var frame = root.querySelector('.tc-frame');
    var frameImg = root.querySelector('.tc-frame-img');
    var lens = root.querySelector('.tc-lens');
    var marksBox = root.querySelector('.tc-marks');
    var regionGroup = root.querySelector('.tc-regionbar .tc-seg');
    var note = root.querySelector('.tc-note');
    var pairOurs = root.querySelector('.tc-pair [data-zoomof="ours"]');
    var pairBaseline = root.querySelector('.tc-pair [data-zoomof="baseline"]');

    var objButtons = all('[data-obj]');
    var modeButtons = all('.tc-toolbar [data-mode]');
    var fullButtons = all('[data-full]');
    var tiles = all('.tc-tile[data-method]');
    var baselineNames = all('.tc-bl-name');
    // Everything that shows the magnified region: the pair and the row.
    var targets = all('[data-zoomof]');

    var first = objButtons[0].getAttribute('data-obj');
    var state = {
      obj: first,
      method: root.getAttribute('data-method'),
      mode: 'regions',
      region: 0,
      full: 'ours',
      u: objects[first].regions[0].at[0],
      v: objects[first].regions[0].at[1]
    };
    var markEls = [];
    var regionButtons = [];

    function src(obj, method) {
      return base + obj + '_' + method + '.webp';
    }

    function clamp(x, lo, hi) {
      return Math.max(lo, Math.min(hi, x));
    }

    function press(buttons, active) {
      buttons.forEach(function (b) {
        b.setAttribute('aria-pressed', b === active ? 'true' : 'false');
      });
    }

    function tileFor(key) {
      return tiles.filter(function (t) {
        return t.getAttribute('data-method') === key;
      })[0];
    }

    function methodName(key) {
      return tileFor(key).querySelector('.tc-tile-name').textContent;
    }

    // Warm the cache for every method of the current object, so switching
    // the comparison is instant rather than a visible reload.
    var warmed = {};
    function warm(obj) {
      ['ours'].concat(tiles.map(function (t) {
        return t.getAttribute('data-method');
      })).forEach(function (m) {
        var url = src(obj, m);
        if (!warmed[url]) {
          warmed[url] = new Image();
          warmed[url].src = url;
        }
      });
    }

    // Lens geometry. The lens is the square of the rendering that the pair
    // shows at ZOOM; every target (pair and row) is scaled so that exactly
    // that square fills it, centered on the same point. This relies on all
    // methods sharing one camera and framing.
    function centerOf(size, w, h, u, v) {
      return [clamp(u * w, size / 2, w - size / 2), clamp(v * h, size / 2, h - size / 2)];
    }

    var queued = false;
    function draw() {
      queued = false;
      var w = frame.clientWidth;
      var h = frame.clientHeight;
      var pw = pairOurs.clientWidth;
      if (!w || !h || !pw) {
        return;
      }

      var size = Math.min(w, h, pw / ZOOM);
      var c = centerOf(size, w, h, state.u, state.v);

      lens.style.width = size + 'px';
      lens.style.height = size + 'px';
      lens.style.transform = 'translate(' + (c[0] - size / 2) + 'px,' + (c[1] - size / 2) + 'px)';

      targets.forEach(function (el) {
        var tw = el.clientWidth;
        var th = el.clientHeight;
        if (!tw) {
          return;
        }
        var k = tw / size;
        el.style.backgroundSize = w * k + 'px ' + h * k + 'px';
        el.style.backgroundPosition = tw / 2 - c[0] * k + 'px ' + (th / 2 - c[1] * k) + 'px';
      });

      objects[state.obj].regions.forEach(function (r, i) {
        var m = markEls[i];
        var p = centerOf(size, w, h, r.at[0], r.at[1]);
        m.style.width = size + 'px';
        m.style.height = size + 'px';
        m.style.transform = 'translate(' + (p[0] - size / 2) + 'px,' + (p[1] - size / 2) + 'px)';
        m.classList.toggle('is-active', state.mode === 'regions' && i === state.region);
      });
    }

    function redraw() {
      if (!queued) {
        queued = true;
        window.requestAnimationFrame(draw);
      }
    }

    function showImages() {
      var shown = state.full === 'ours' ? 'ours' : state.method;
      frameImg.src = src(state.obj, shown);
      frameImg.alt = objects[state.obj].name + ', front view, textured by ' +
        (state.full === 'ours' ? 'our method' : methodName(state.method));
      targets.forEach(function (el) {
        var of = el.getAttribute('data-zoomof');
        el.style.backgroundImage = 'url("' + src(state.obj, of === 'baseline' ? state.method : of) + '")';
      });
      redraw();
    }

    function setRegion(i) {
      var r = objects[state.obj].regions[i];
      state.region = i;
      state.u = r.at[0];
      state.v = r.at[1];
      press(regionButtons, regionButtons[i]);
      redraw();
    }

    // Region buttons and on-image outlines are rebuilt for each object.
    function buildRegions() {
      marksBox.textContent = '';
      regionGroup.textContent = '';
      markEls = [];
      regionButtons = [];
      objects[state.obj].regions.forEach(function (r, i) {
        var mark = document.createElement('button');
        mark.type = 'button';
        mark.className = 'tc-mark';
        mark.setAttribute('aria-label', 'Region ' + (i + 1) + ': ' + r.name);
        mark.innerHTML = '<span class="tc-mark-num">' + (i + 1) + '</span>';
        mark.addEventListener('click', function () {
          setRegion(i);
        });
        marksBox.appendChild(mark);
        markEls.push(mark);

        var b = document.createElement('button');
        var num = document.createElement('span');
        b.type = 'button';
        num.className = 'tc-rnum';
        num.textContent = i + 1;
        b.appendChild(num);
        b.appendChild(document.createTextNode(r.name));
        b.addEventListener('click', function () {
          setRegion(i);
        });
        regionGroup.appendChild(b);
        regionButtons.push(b);
      });
    }

    function setObject(key) {
      state.obj = key;
      press(objButtons, objButtons.filter(function (b) {
        return b.getAttribute('data-obj') === key;
      })[0]);
      buildRegions();
      setRegion(0);
      warm(key);
      showImages();
    }

    function setMode(mode) {
      state.mode = mode;
      root.setAttribute('data-mode', mode);
      press(modeButtons, modeButtons.filter(function (b) {
        return b.getAttribute('data-mode') === mode;
      })[0]);
      if (mode === 'regions') {
        setRegion(state.region);
      }
      redraw();
    }

    function setMethod(key) {
      state.method = key;
      press(tiles, tileFor(key));
      baselineNames.forEach(function (el) {
        el.textContent = methodName(key);
      });
      pairBaseline.setAttribute('aria-label', 'Magnified ' + methodName(key));
      note.textContent = tileFor(key).getAttribute('data-note');
      showImages();
    }

    function pointAt(e) {
      var r = frame.getBoundingClientRect();
      state.u = clamp((e.clientX - r.left) / r.width, 0, 1);
      state.v = clamp((e.clientY - r.top) / r.height, 0, 1);
      root.classList.add('tc-touched');
      redraw();
    }

    // Free zoom: the mouse follows on hover; touch and pen place on tap and
    // follow while dragging. Highlights ignore the pointer except the marks.
    frame.addEventListener('pointermove', function (e) {
      if (state.mode === 'free' && (e.pointerType === 'mouse' || e.buttons)) {
        pointAt(e);
      }
    });
    frame.addEventListener('pointerdown', function (e) {
      if (state.mode === 'free') {
        pointAt(e);
      }
    });

    frame.addEventListener('keydown', function (e) {
      var n = objects[state.obj].regions.length;
      if (state.mode === 'regions') {
        var dir = { ArrowLeft: -1, ArrowUp: -1, ArrowRight: 1, ArrowDown: 1 }[e.key];
        if (dir) {
          e.preventDefault();
          setRegion((state.region + dir + n) % n);
        }
        return;
      }
      var step = e.shiftKey ? 0.08 : 0.02;
      var d = { ArrowLeft: [-step, 0], ArrowRight: [step, 0], ArrowUp: [0, -step], ArrowDown: [0, step] }[e.key];
      if (d) {
        e.preventDefault();
        state.u = clamp(state.u + d[0], 0, 1);
        state.v = clamp(state.v + d[1], 0, 1);
        root.classList.add('tc-touched');
        redraw();
      }
    });

    objButtons.forEach(function (b) {
      b.addEventListener('click', function () {
        setObject(b.getAttribute('data-obj'));
      });
    });

    modeButtons.forEach(function (b) {
      b.addEventListener('click', function () {
        setMode(b.getAttribute('data-mode'));
      });
    });

    fullButtons.forEach(function (b) {
      b.addEventListener('click', function () {
        state.full = b.getAttribute('data-full');
        press(fullButtons, b);
        showImages();
      });
    });

    tiles.forEach(function (t) {
      t.addEventListener('click', function () {
        setMethod(t.getAttribute('data-method'));
      });
    });

    frameImg.addEventListener('load', redraw);
    window.addEventListener('resize', redraw);
    if (window.ResizeObserver) {
      new ResizeObserver(redraw).observe(root);
    }

    setObject(state.obj);
    setMethod(state.method);
  })();
</script>

<script type="importmap">
  {
    "imports": {
      "three": "https://cdn.jsdelivr.net/npm/three@0.169.0/build/three.module.js",
      "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.169.0/examples/jsm/"
    }
  }
</script>

<script type="module">
  // Split-view 3D comparison of the vase. The same mesh is drawn twice per
  // frame through one camera: Blended left of the divider, Ours right of it.
  // three.js and the model load only when the section comes near the viewport.
  const root = document.getElementById('tc-3d');

  // Opening view: the camera chosen for the code release's README figure,
  // shifted into the frame of the centered mesh in vase.bin.
  const HOME = [0.0451, 0.9289, 3.5627];
  const FOV = 30;

  if (root) {
    const io = new IntersectionObserver((entries) => {
      if (entries.some((e) => e.isIntersecting)) {
        io.disconnect();
        init().catch((err) => {
          console.error(err);
          root.querySelector('.tc-3d-status').textContent = 'The 3D viewer could not load in this browser.';
        });
      }
    }, { rootMargin: '300px' });
    io.observe(root);
  }

  async function init() {
    const [THREE, { OrbitControls }] = await Promise.all([
      import('three'),
      import('three/addons/controls/OrbitControls.js'),
    ]);

    const base = root.getAttribute('data-base');
    const stage = root.querySelector('.tc-3d-stage');
    const canvas = root.querySelector('.tc-3d-canvas');
    const divider = root.querySelector('.tc-3d-divider');
    const status = root.querySelector('.tc-3d-status');
    const reset = root.querySelector('.tc-3d-reset');
    const tapHint = root.querySelector('.tc-3d-tap');

    const renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 2));
    renderer.setClearColor(0xffffff, 1);

    // vase.bin: uint32 vertex count, uint32 index count, then float32
    // positions (xyz), float32 uvs, uint16 triangle indices.
    const loadTexture = (url) => new Promise((resolve, reject) => {
      new THREE.TextureLoader().load(url, resolve, undefined, reject);
    });
    const [buf, texBlended, texOurs] = await Promise.all([
      fetch(base + 'vase.bin').then((r) => {
        if (!r.ok) throw new Error('vase.bin: HTTP ' + r.status);
        return r.arrayBuffer();
      }),
      loadTexture(base + 'blended.webp'),
      loadTexture(base + 'ours.webp'),
    ]);
    const [nV, nI] = new Uint32Array(buf, 0, 2);
    const geometry = new THREE.BufferGeometry();
    geometry.setAttribute('position', new THREE.BufferAttribute(new Float32Array(buf, 8, nV * 3), 3));
    geometry.setAttribute('uv', new THREE.BufferAttribute(new Float32Array(buf, 8 + nV * 12, nV * 2), 2));
    geometry.setIndex(new THREE.BufferAttribute(new Uint16Array(buf, 8 + nV * 20, nI), 1));

    // Unlit, so the textures appear exactly as computed.
    const material = (tex) => {
      tex.colorSpace = THREE.SRGBColorSpace;
      tex.anisotropy = renderer.capabilities.getMaxAnisotropy();
      return new THREE.MeshBasicMaterial({ map: tex, side: THREE.DoubleSide });
    };
    const matBlended = material(texBlended);
    const matOurs = material(texOurs);

    const scene = new THREE.Scene();
    const mesh = new THREE.Mesh(geometry, matOurs);
    scene.add(mesh);

    const camera = new THREE.PerspectiveCamera(FOV, 1, 0.05, 50);
    const controls = new OrbitControls(camera, canvas);
    controls.enablePan = false;
    controls.minDistance = 1.0;
    controls.maxDistance = 9;

    function goHome() {
      camera.position.set(HOME[0], HOME[1], HOME[2]);
      camera.up.set(0, 1, 0);
      controls.target.set(0, 0, 0);
      controls.update();
    }

    let split = 0.5;
    function render() {
      const w = canvas.clientWidth;
      const h = canvas.clientHeight;
      const sx = Math.round(w * split);
      renderer.setScissorTest(true);
      mesh.material = matBlended;
      renderer.setScissor(0, 0, sx, h);
      renderer.render(scene, camera);
      mesh.material = matOurs;
      renderer.setScissor(sx, 0, w - sx, h);
      renderer.render(scene, camera);
    }

    function resize() {
      const w = stage.clientWidth;
      const h = stage.clientHeight;
      if (!w || !h) return;
      renderer.setSize(w, h, false);
      camera.aspect = w / h;
      camera.updateProjectionMatrix();
      render();
    }

    function setSplit(s) {
      split = Math.min(0.98, Math.max(0.02, s));
      divider.style.left = split * 100 + '%';
      divider.setAttribute('aria-valuenow', String(Math.round(split * 100)));
      render();
    }

    controls.addEventListener('change', render);
    new ResizeObserver(resize).observe(stage);

    // Divider: drag it, or focus it and use the arrow keys.
    divider.addEventListener('pointerdown', (e) => {
      divider.setPointerCapture(e.pointerId);
      e.preventDefault();
    });
    divider.addEventListener('pointermove', (e) => {
      if (divider.hasPointerCapture(e.pointerId)) {
        const r = stage.getBoundingClientRect();
        setSplit((e.clientX - r.left) / r.width);
      }
    });
    divider.addEventListener('keydown', (e) => {
      const step = e.shiftKey ? 0.1 : 0.02;
      const d = { ArrowLeft: -step, ArrowRight: step, Home: -1, End: 1 }[e.key];
      if (d !== undefined) {
        e.preventDefault();
        setSplit(split + d);
      }
    });

    reset.addEventListener('click', () => {
      goHome();
      setSplit(0.5);
    });

    // Touch screens: the model only takes touches after a tap, so a swipe
    // across it still scrolls the page. Touching elsewhere releases it.
    if (window.matchMedia('(pointer: coarse)').matches) {
      const activate = (on) => {
        controls.enabled = on;
        canvas.style.touchAction = on ? 'none' : 'pan-y';
        tapHint.hidden = on;
      };
      activate(false);
      // Anywhere in the stage: the hint sits over the divider, so a tap on
      // the hint lands on the divider rather than the canvas.
      stage.addEventListener('click', () => activate(true));
      document.addEventListener('pointerdown', (e) => {
        if (!stage.contains(e.target)) activate(false);
      });
    }

    goHome();
    setSplit(0.5);
    resize();
    status.hidden = true;
    divider.hidden = false;
    reset.hidden = false;
    root.querySelectorAll('.tc-tag').forEach((t) => { t.hidden = false; });
    root.classList.add('is-ready');
  }
</script>
