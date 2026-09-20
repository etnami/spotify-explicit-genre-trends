# The Spotify Effect: Genre and Explicit Content Trends in the Streaming Age

MSc Data Science coursework (University of Sheffield), Grade: 65 (Merit). A 6-chart data visualization composite exploring whether the proportion of explicit content in popular music changed after Spotify's 2008 launch, and which genres drove that shift, built from the MusicOSet dataset (20,405 songs, filtered to 2000-2018).

> **Reproducibility note:** the underlying MusicOSet CSVs aren't included in this repo (see Data, below), so `spotify_genre_explicit_viz.Rmd` can't be rerun end-to-end from this repo alone. 5 of the 6 figures below are the actual final chart images, extracted directly from the coursework's own write-up (not regenerated or redrawn); the 6th (the grouped bar chart) was supplied separately as a higher-resolution export. What you see is exactly what the code produced, not a redraw. Numbers in this README were cross-checked against what's visible in these images and the write-up's discussion, not independently recomputed, since the raw data isn't available to rerun.

## Key results

**Did explicit content increase after Spotify's 2008 launch?** Yes, modestly overall: **15% of popular songs were explicit pre-Spotify (2000-2007) vs. 20% post-Spotify (2008-2018)**, a 5 percentage point increase.

![Waffle chart: explicit content pre vs post Spotify](figures/explicit_content_waffle_pre_post_spotify.png)

That 15%/20% split is against a backdrop of rapidly growing overall output: total releases grew from 2,133 (pre-Spotify) to 3,427 (post-Spotify), and total popular songs from 857 to 1,221. Within that growth, explicit-and-popular songs grew from 127 to 240, while explicit-but-not-popular songs grew far faster, from 179 to 537. These totals are exactly the sums of the year-by-year line chart further down (127 and 179 are the exact sums of the "Popular" and "Not Popular" series for 2000-2007; 240 and 537 for 2008-2018), so the two charts independently confirm each other.

![Grouped bar: volume, explicitness and popularity by era](figures/volume_explicit_popularity_by_era_bar.png)

**Which genres drove the shift?** Overwhelmingly one: `atl hip hop`. Across the top 5 genres by popular-song volume, most stayed flat or low (contemporary country: 0-1% explicit throughout; alternative rock and alternative metal fluctuating in the 0-18% range), but ATL hip hop climbed from 33% (2000-2003) to 100% (2016-2018), the only genre showing a clear, sustained upward trend through the whole period.

![Heatmap: explicit content share by genre and time period](figures/explicit_content_heatmap_top5_genres.png)

**Case study, ATL hip hop in 2018 specifically:** 92% of ATL hip hop songs released that single year were explicit. This is a related but distinct number from the heatmap's "100%" figure above, that 100% describes the full 2016-2018 bin, not 2018 in isolation; see Verification note.

![Waffle chart: ATL hip hop explicitness, 2018 only](figures/atl_hiphop_2018_case_study_waffle.png)

**Which genres convert output into hits most efficiently?** Among the top 5 genres by popular-song volume, alternative metal has the highest hit rate (44% of everything the genre releases becomes popular), narrowly ahead of contemporary country (43%) and alternative rock (43%). ATL hip hop, despite dominating the explicit-content story, has the *lowest* hit rate of the five (35%), it produces a large volume of popular songs mainly by releasing a lot of music, not by having an unusually high hit rate.

![Stacked bar: popular vs non-popular share by genre](figures/hit_to_release_ratio_stacked_bar.png)

**How did raw volume change over time?** The line chart is the one place the composite's own write-up flags a real interpretive risk: non-popular explicit songs explode from 29 (2015) to 171 (2018), a volume increase so large it can make explicit content look like it's "winning" even though the *popular* explicit count only grows from 26 to 47 in the same window. The write-up's own ethical-implications section calls this out directly: read on volume alone, this chart risks implying the streaming transition had minimal effect on what became popular, when the other four charts show otherwise.

![Line chart: explicit song volume, popular vs non-popular, 2000-2018](figures/explicit_song_volume_timeseries.png)

## Methods & tools

- **Language:** R (R Markdown), `ggplot2`, `tidyverse`
- **Data:** MusicOSet (Silva et al., 2019), 4 subsets (songs, artists, acoustic features, popularity) merged via `left_join`, filtered to 2000-2018
- **Chart types:** waffle charts (`geom_tile` on an `expand.grid` waffle layout, `facet_wrap` for side-by-side era comparison), a heatmap (`geom_tile` with `geom_text` percentage labels), a 100% stacked bar chart, a time series with direct data labels
- **Design framework:** built and justified using the ASSERT framework (Ferster, 2013), the write-up documents Ask/Search/Structure/Envision/Represent/Tell for each chart, plus a dedicated accessibility section (colour-blind-safe `cividis` palette, high-contrast text labels, dyslexia-friendly sans-serif font)

## Repo structure

```
05-spotify-genre-explicit-trends/
├── README.md                        ← you are here
├── spotify_genre_explicit_viz.Rmd  ← full visualisation code
└── figures/
    ├── explicit_content_waffle_pre_post_spotify.png
    ├── volume_explicit_popularity_by_era_bar.png
    ├── explicit_content_heatmap_top5_genres.png
    ├── atl_hiphop_2018_case_study_waffle.png
    ├── hit_to_release_ratio_stacked_bar.png
    └── explicit_song_volume_timeseries.png
```

## How to run

1. Install R and the packages the script loads at the top (`tidyverse`, `ggplot2`, `dplyr`).
2. The script expects 4 pre-cleaned CSVs (`songs cleaned converted copy.csv`, `artists for vis cleaned.csv`, `acoustic features cleaned converted copy.csv`, `song ispop cleaned converted copy.csv`) that aren't included in this repo; see Data, below.
3. Run top to bottom; the script merges the 4 sources, filters to 2000-2018, then produces all 6 chart blocks in sequence, matching all 6 figures in this README.

## Data

MusicOSet (Silva et al., 2019) is a public dataset, but the pre-cleaned CSVs this script reads are not redistributed here. The 6 figures in this repo are the actual final chart images (5 extracted from the coursework's write-up, 1 supplied as a separate export), standing in for a live rerun.

## Limitations

- No raw data or code-execution outputs were available to independently verify these numbers from scratch; verification here means checking that the write-up's stated numbers match what's actually visible in the final chart images, not recomputing them from source data.
- The line chart (last figure above) is flagged in the original write-up itself as ethically risky to read on its own, since raw-count growth in non-popular explicit songs can visually overwhelm the smaller, but real, growth in popular explicit songs. That caveat is preserved here rather than smoothed over.
- The heatmap groups years into 4-year bins (2000-2003, 2004-2007, 2008-2011, 2012-2015, 2016-2018) rather than showing single years, which is why its "100%" figure for ATL hip hop and the dedicated single-year case study's "92%" figure both correctly coexist; see Verification note.

## Verification note

The write-up's text states that "by 2018, 100% of popular songs in the ATL hip-hop genre featured explicit content," citing the heatmap. Looking at the heatmap directly, that 100% figure is real, but it's the value for the **2016-2018 bin as a whole**, not 2018 specifically. The dedicated single-year case-study chart, built specifically to zoom in on 2018 alone, shows **92%** for that year. Both numbers are correctly computed outputs of their respective charts; the write-up's prose just states the binned figure using language ("by 2018") that reads as if it were the single-year figure. This README reports both numbers separately and labels which is which, rather than repeating the write-up's "100% by 2018" framing as if it were the 2018-only value.

Separately, the grouped bar chart's totals (127/179 explicit-popular/explicit-non-popular pre-Spotify, 240/537 post-Spotify) were independently checked by summing the corresponding year-by-year values from the line chart. Both pairs matched exactly, a genuine cross-check between two independently-built charts, not just a repetition of the same aggregation.
