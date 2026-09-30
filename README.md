# Optometry Visualizer

A single-page, dependency-free simulator of what an *uncorrected* eye sees through a given eyeglass prescription. Enter sphere, cylinder and axis (plus- or minus-cyl form), pick a pupil size and how hard the eye is accommodating, and compare corrected vs. uncorrected views of:

- a Snellen-style eye chart drawn at true angular sizes (20/20 letter = 5 arcmin),
- a built-in landscape scene with a fence, road signs and text (good for seeing directional astigmatic blur),
- any photo you upload (stays in your browser; nothing is sent anywhere).

Everything is in `index.html`; the `brand/` folder and favicons carry the Cardone Medical Consulting teal brand (deep teal #145C60, sea glass #A7CEC7, ivory #F5F3EC). No build step, no dependencies. Needs a browser with WebGL (any modern desktop or phone browser).

## Run it

Open `index.html` in a browser, or host it on GitHub Pages:

1. In GitHub Desktop: **File → Add local repository…** → pick this folder → **Publish repository** (name `optometry-visualizer`, uncheck "Keep this code private" so Pages is free).
2. On github.com open the repo → **Settings → Pages** → under *Build and deployment* choose **Deploy from a branch**, branch `main`, folder `/ (root)` → **Save**.
3. After a minute the site is live at `https://<your-username>.github.io/optometry-visualizer/`. If your user site (`<your-username>.github.io`) has a custom domain, the same page is also served under that domain, e.g. `https://projects.example.com/optometry-visualizer/`.

You can pre-fill a prescription with URL parameters: `index.html?s=-0.25&c=2.5&a=180`.

## How it works

The prescription is split into its two principal meridians (power along the axis = sphere; power 90° away = sphere + cylinder). Any accommodation is subtracted from both. Each meridian's leftover defocus *D* (diopters) spreads a point of light into a streak of angular length ≈ pupil diameter × |D| (e.g. 4 mm × 2.5 D → 0.01 rad ≈ 34 arcmin). The two streaks form an elliptical blur patch oriented along the meridians, which a WebGL fragment shader convolves with the image. Pixels are converted to arcminutes using the chart's fixed scale or the "field of view" slider for scenes/photos.

The accommodation buttons let you explore what a young, strongly accommodating eye can do: it can neutralize hyperopia, but with astigmatism it can only bring *one* meridian into focus at a time; the other stays blurred by the full cylinder value.

## Limits and calibration

Pure geometric optics overstates how blurry things *look* to the person: it ignores diffraction and higher-order aberrations, and above all neural adaptation, which makes habitual blur feel sharper than a blurred picture looks to a normally sighted viewer. So the simulator applies Stiles–Crawford weighting across the pupil, defaults to a 3 mm pupil (a bright exam room), and draws the blur at 0.7× the geometric size by default; a "Blur scale" slider lets you see pure ray optics at 1.0. The 20/xx readout is a separate empirical fit to published uncorrected-acuity data (roughly 1 D → 20/60, 2 D → 20/135, 3 D → 20/220), not a measurement from the picture. Infants and toddlers also have immature acuity even with perfect correction. This is an educational tool, not medical advice.

## License

Code is MIT. The Cardone Medical Consulting name, logo and brand assets in `brand/` and the favicons are © 2026 Cardone Medical Consulting, LLC and are not covered by the MIT license.
