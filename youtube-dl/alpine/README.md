## cdituri/openconnect

`cdituri/youtube-dl` an alpine-based container for youtube-dl containing aria2.

Please see https://rg3.github.io/youtube-dl/ for more information on youtube-dl.

Please see https://aria2.github.io/ for more information on aria2.


### building
```
docker build -t cdituri/youtube-dl .
```

### examples

Minimal suggested invocation:

```
docker run -it --rm -v $HOME/Music:/data -u $UID:$UID cdituri/youtube-dl <youtube-url>
```

Extract mp3 audio from youtube url:

```
docker run -it --rm -v $HOME/Music:/data -u $UID:$UID cdituri/youtube-dl -x --audio-format mp3 <youtube-url>
```

Same as above but speedup with aria2 as external downloader, see https://github.com/rg3/youtube-dl/issues/15384#issuecomment-359654155:

```
docker run -it --rm -v $HOME/Music:/data -u $UID:$UID cdituri/youtube-dl -x --audio-format mp3 --external-downloader aria2c --external-downloader-args "-x 16 -s 16 -k 1M" <youtube-url>
```
