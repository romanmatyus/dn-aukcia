# Návod na prevod detailného návrhu do PDF

## Inštalácia prostredia

	$ sudo apt-get install dvipdfmx haskell-platform nbibtex texlive-latex-base texlive-latex-recommended texlive-latex-extra preview-latex-style dvipng texlive-fonts-recommended
	$ cabal update
	$ cabal install pandoc

## Prevod do PDF

	$ ~/.cabal/bin/pandoc -N --template=template.tex --variable mainfont=Georgia --variable sansfont=Arial --variable monofont="DejaVu Sans Mono" --variable fontsize=12pt --variable lang=slovak DN-Dru-Aukcie.md --latex-engine=xelatex --toc -o DN-Dru-Aukcie.pdf
