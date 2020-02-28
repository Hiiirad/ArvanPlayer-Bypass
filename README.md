# Download a video embedded in website under ArvanCloud protection (ArvanPlayer)

## Introduction
I have a habit (some say lousy habit) that I have to download every video that I want to watch online, because of the low bandwidth that I have at home.

Recently, I found a tutorial video for "Bash & BugBounty" in 2 parts. ([Part 1](https://memoryleaks.ir/bug-bounty-and-bash-part-1/) & [Part 2](https://memoryleaks.ir/bug-bounty-and-bash-part-2/))

These videos, at first sight, aren't downloadable and protected by ArvanCloud services such as WAF, etc.

It made me mad and frustrated about this action, so I have to come up with an idea to calm myself down. In other words, I had to download those videos. :)

After a long time banging my head to keyboard and thinking in every way, I finally found a way :)

## Process
Let's get into it:

1. I analyzed source code using inspect element from browser looking for `iframe src=` or `src=`, and then I found a link I was looking for.
Analyze file from Source Page and Inspect Element from browser.
2. First video [link](https://player.arvancloud.com/index.html?config=https://memoryleak.arvanvod.com/kb7yZJyN0g/oRNXrd7kae/origin_config.json) and Second video [link](https://player.arvancloud.com/index.html?config=https://memoryleak.arvanvod.com/kb7yZJyN0g/PeAWr4vmMl/origin_config.json)
3. Then I fired up the inspect element and looked in the network section, and I found a pattern.
4. The video streamed through different segments. So I looked at the first one and the last one and copy their URL.

Example from part 2 of that tutorial:
```
First link:
https://memoryleak.arvanvod.com/kb7yZJyN0g/PeAWr4vmMl/h_,144_200,240_400,360_800,480_1500,720_2500,1080_4366,k.mp4.list/seg-1-f5-v1-a1.ts

Last link:
https://memoryleak.arvanvod.com/kb7yZJyN0g/PeAWr4vmMl/h_,144_200,240_400,360_800,480_1500,720_2500,1080_4366,k.mp4.list/seg-392-f5-v1-a1.ts
```

5. Then I wrote a script to make a download list and use `wget` command to download all segments. (FYI, There were 491 segments in the first video and 392 segments in the second video) (Check the [link-maker.sh](./link-maker.sh))
```bash
bash link-maker.sh > links.txt
wget --tries=0 --wait=5 --random-wait --input-file=links.txt
```
6. After that, I had to concatenate all segments to have a single file. (Check the [concatenate.sh](.concatenate.sh))
```bash
bash concatenate.sh > all.ts
```
7. Finally, I had to convert `.ts` extension to a more convenient one such as `.mp4` with another shell command:
```bash
ffmpeg -i all.ts -acodec copy -vcodec copy final.mp4
```
## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
This project is licensed under the GPL-3.0 License - see the [LICENSE](./LICENSE) file for details.
