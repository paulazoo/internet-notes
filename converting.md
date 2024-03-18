# if `jupyter nbconvert --to pdf Homework05_mine.ipynb` doesn't work
- tlmgr update --self --all
- https://ctan.math.utah.edu/ctan/tex-archive/systems/texlive/tlnet

# 231204 error
- in ./zshrc
```
export JUPYTER_PATH=/opt/homebrew/share/jupyter 
export JUPYTER_CONFIG_PATH=/opt/homebrew/etc/jupyter
```
- jupyter lab
- export as pdf

`pandoc manual.md -o manual.pdf`

# avi to mp4, without re-encoding so don't specify codecs
ffmpeg -i input.avi -c:v copy -c:a copy OUTPUT.mp4