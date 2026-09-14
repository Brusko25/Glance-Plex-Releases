# Release screenshot checklist

For every new program release, include visual previews on the repository homepage and in the GitHub release notes.

- Capture the actual release build. Include a main screen and, where useful, two or more distinct feature views.
- Use an isolated example workspace; keep account details, notification topics, credentials and personal holdings out of public images. Label sample data and offline previews.
- Save PNGs under `images/<version>/` with readable names and describe the capture version and source in that folder's README.
- Keep previous versions' images unchanged. Reuse a prior image only after verifying that the interface is unchanged, and identify the captured version accurately.
- Update the homepage's Screenshots section near the download introduction. Include short captions, descriptive alt text and clickable full-size images.
- Add the version's screenshots to its release notes using absolute public image URLs pinned to the documentation commit SHA. Relative image paths do not reliably resolve in release notes.
- Before publishing, visually inspect every image and check that all image links load without a GitHub login.
- Preserve existing release notes, downloads, tags and update metadata. Do not attach current screenshots to old releases as though they depict the old build.
