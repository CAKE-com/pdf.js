If there is an issue with pdf preview on webclient it needs to be fixed in this project.

## Local development

1. Make changes and run `npx gulp minified`. Output files are placed in `build/minified/` folder.
2. With webclient dev server running, in the monorepo run either of the two commands:
   - `nx run webclient:get-pdfjs -- --localPath=../../../<local-path-to-this-repo>/build/minified`
   - `node apps/webclient/tools/get-pdfjs-dist.js --localPath=<local-path-to-this-repo>/build/minified`
3. Now you can re-open a pdf to test the changes
   - Make sure cache is disabled in devtools Network tab

## Release process

Once fix/change is merged to master we need to create a new release.

To create new release go to github **Releases** section and **Draft a new release**

Put release name with a short description of changes and bump tag with next patch version. If it's just latest release from pdf.js repo that are being merged, use the corresponding release tag that you merged.

Create a zip and upload it in binaries section:

- Run `npx gulp minified`
- Output files are placed in `build/minified/` folder
- Copy `minified/` somewhere, rename to `pdfjs-dist` and zip it using command line tools
  - Example: `zip -d v4.2.67.zip __MACOSX/\*`
    - name the zip file after the release tag, eg `v4.2.67.zip`
    - on MacOS, zip will include `__MACOSX` folder by default, so you need to explicitly it

(☝️ This can probably be automated through Gulp file)

Now submit the release.

Test it by running `nx run webclient:get-pdfjs` in the monorepo - it should fetch the latest release assets and place them in the `pdfjs-dist` folder. **These are the assets that must be committed**.
