

```
pandoc assets/document.md -o assets/document.pdf --template assets/eisvogel.tex --listings
```


```
name: markdown2pdf

on: push

jobs:
  convert_via_pandoc:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: docker://pandoc/extra:3.5
        with:
          args: assets/document.md --output=output/document.pdf --template assets/eisvogel.latex --listings --number-sections -V block-headings 
      - uses: actions/upload-artifact@v4
        with:
          name: output
          path: output
      - name: Commit files # transfer the new pdf files back into the repository
        run: |
          git config --local user.name "Kevin-Mattheus-Moerman"
          git add .
          git commit -m "Updating pdfs"
      - name: Push changes # push the output folder to your repo
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          force: true
  ```