# EXIF Viewer & Metadata Stripper

See every piece of metadata hidden in a photo — camera, lens, exposure, GPS location, timestamps — then strip it out losslessly. The file never leaves your browser.

**Live:** <https://exif-viewer.slippylabs.com/>

## What it does

- Every tag hiding in a photo — camera body, lens, exposure settings, software, serial number, timestamps.
- GPS coordinates decoded to something you can read, when the photo carries them.
- Reads JPEG, PNG, WebP and TIFF, walking the real IFD structure rather than guessing.
- One button to strip the lot back out — optionally keeping the ICC colour profile and the orientation tag.

## How it works

Stripping is **lossless**. The tool rebuilds the container around the untouched compressed image data — dropping the APP1..APP15 and comment segments from a JPEG, the text and eXIf chunks from a PNG, the EXIF and XMP chunks from a WebP — rather than decoding the picture to a canvas and re-encoding it. Re-encoding is what most 'metadata removers' do, and it quietly costs you image quality every time.

The fiddly part is WebP: its `VP8X` header carries flags declaring which optional chunks exist, so removing the EXIF chunk without clearing the corresponding flag leaves a file that sends readers looking for a chunk that is no longer there. Those bits get cleared.

Keeping orientation is offered separately because it is the one tag you usually want to survive — strip it along with everything else and a phone photo comes back rotated.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/exif-viewer.slippylabs.com.git
cd exif-viewer.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## Notes

The photo is read with `FileReader` and parsed in the page. It is never uploaded — which matters more here than in most tools, because the tags you are trying to see are the ones you probably do not want on someone else's server.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
