The bash .sh file to find all pdf in current directory that has less that 3 fonts installed within it. Then move these pdfs to a subfolder named unscanned.

For next step, we can use Acrobat to do OCR over the subfolder.

```bash
#!/bin/zsh 
#set -vx
for file in *.pdf
do
    name=${file##*/}
    base=${name%.pdf}
        if ((`pdffonts "$name" | wc -l` < 5 )); then
		echo "$name" "-> Not Searchable" 
		mv "$name" "./unscanned/"
	else
		echo "$name"
	fi
done
echo "Done"
```
