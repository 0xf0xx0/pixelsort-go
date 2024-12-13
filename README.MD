# pixelsort_go
pixelsorter cli written in golang

rewritten from my nodejs implementation, which used [satyarth/pixelsort](https://github.com/satyarth/pixelsort/) as reference

furst time using go, might be cursed

## features
- row and spiral patterns
- sort with a mask
- sort images in parallel
- sort by lightness, hue, saturation, and r/g/b
- shuffle pixels, sort in waves, random lengths, or smear instead
- sort in reverse
- rotation

## wanted features
- would like to mix and match thresholds (sort by red with saturation threshold, or sort by sat with darkness threshold)
- more patterns; seam carving looks cool
- completion? im lazy

## install
### from source
```sh
git clone https://github.com/0xf0xx0/pixelsort-go.git
cd pixelsort-go
go build
go install
```

## examples/usage
**Original**
![webb picture-of-the-month 2023/10](https://cdn.esawebb.org/archives/images/screen/potm2310a.jpg) - [src: ESA/Webb, NASA & CSA, A. Adamo (Stockholm University) and the FEAST JWST team](https://esawebb.org/images/potm2310a/)

**Mask**
![mask](./examples/mask.jpg)

**Random sort**
`pixelsort_go --input ~/Downloads/potm2310a.jpg --upper_threshold 0.4 --lower_threshold 0.2 --comparator saturation --angle 30 --randomness 0.4 --interval randomnoisy --output ./examples/webb-sort.jpg`
![randomsort](./examples/webb-random-angled.jpg)

**Basic row sort w/ default options & mask**
`pixelsort_go --input ~/Downloads/potm2310a.jpg --mask ./examples/webb-mask.jpg --output ./examples/webb-sort.jpg`
![masked row sort](./examples/webb-row-masked.jpg)

**Spiral sort using most options & mask**
`pixelsort_go --input ~/Downloads/potm2310a.jpg --mask ./examples/webb-mask.jpg --pattern spiral --interval wave --randomness 0.4 --comparator saturation --section_length 400 --output ./examples/webb-spiral.jpg`
![masked spiral sort](./examples/webb-spiral-masked.jpg)
did you know webb and hubble pics are cc4?

## "benchmark"
"elapsed" is time spent actually sorting
src img is "large jpg" from [esawebb](https://esawebb.org/images/potm2310a/)
```
=> time pixelsort_go -i ~/Downloads/potm2310a.jpg -o ./benchmark
Sorting 1 images with a config of {Pattern:row Interval:row Comparator:lightness SectionLength:69 Randomness:1 Reverse:false Thresholds:{Lower:0 Upper:1} Angle:0}.
Loading image 1 (/home/________/Downloads/potm2310a.jpg -> ./benchmark)...
Sorting /home/________/Downloads/potm2310a.jpg...
./benchmark elapsed: 2.014s
Writing ./benchmark...
0:02.68 real 2.64 usermode 99% 93240 Kb mem

=> time pixelsort_go -i ~/Downloads/potm2310a.jpg -m ./examples/mask.jpg -o ./benchmark
Sorting 1 images with a config of {Pattern:row Interval:row Comparator:lightness SectionLength:69 Randomness:1 Reverse:false Thresholds:{Lower:0 Upper:1} Angle:0}.
Loading image 1 (/home/________/Downloads/potm2310a.jpg -> ./benchmark)...
Sorting /home/________/Downloads/potm2310a.jpg...
./benchmark elapsed: 1.068s
Writing ./benchmark...
0:02.12 real 2.01 usermode 99% 215344 Kb mem
```
