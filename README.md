# Fusion Media Center Settings

Configuration for [AIOStreams](https://github.com/Viren070/AIOStreams) formatter and [Fusion](https://github.com/42degrees/fusion) filters.

## Filters

### AIOStreams Name

Converts the nSeScore (Stremio Enhanced seal score) star rating into symbols, then appends the stream quality label.

```
{stream.nSeScore::exists["{stream.nSeScore::pstar::replace('⯪','★')::replace('★★★★★','♛ ')::replace('★★★★☆','⭑ ')::replace('★★★☆☆','✦ ')::replace('★★☆☆☆','△ ')::replace('★☆☆☆☆','∅ ')::replace('☆☆☆☆☆','∅ ')}"||""]}{stream.quality::exists["{stream.quality::title}"||""]}
```

Symbol mapping:

| Stars | Symbol | Meaning |
|-------|--------|---------|
| ★★★★★ | ♛ | Best |
| ★★★★☆ | ⭑ | Good |
| ★★★☆☆ | ✦ | Good |
| ★★☆☆☆ | △ | OK |
| ★☆☆☆☆ | ∅ | OK |
| ☆☆☆☆☆ | ∅ | OK |

### AIOStreams Description

Shows filename on the first line, then service shortname, stream type, and file size on the second line.

```
{stream.filename::exists["{stream.filename}"||""]}\n{service.shortName::exists["{service.shortName}"||""]}{stream.type::exists[" · {stream.type::title::replace('P2p','P2P')}"||""]}{stream.size::>0[" · {stream.size::bytes}"||""]}
```

### Fusion Filters Settings

Import the following URL into Fusion's Settings → Filters:

```
https://raw.githubusercontent.com/tenhobi/fusion-settings/main/filters.json
```

---

## Filter Groups

Filters use the symbols from the Name template and standard release tag keywords to categorize streams. Each group is displayed as a separate row of tags in Fusion.

### Quality (`gq`)

Combines the nSeScore symbol with the source format. Ordered from best to worst so higher-quality matches take priority.

| Filter | Matches |
|--------|---------|
| Best Remux | ♛ + remux |
| Best BluRay | ♛ + bluray/blu-ray, not remux |
| Best WebDL | ♛ + web-dl/webdl/webrip |
| Good Remux | ⭑ or ✦ + remux |
| Good BluRay | ⭑ or ✦ + bluray/blu-ray, not remux |
| Good WebDL | ⭑ or ✦ + web-dl/webdl/webrip |
| OK Remux | △ or ∅ + remux |
| OK BluRay | △ or ∅ + bluray/blu-ray, not remux |
| OK WebDL | △ or ∅ + web-dl/webdl/webrip |
| SeaDex | seadex / best release / alt release tags |

### Resolution (`gr`)

| Filter | Matches |
|--------|---------|
| 4K | 2160p/4K/UHD, excludes 1080p and 720p |
| 1080p | 1080p or 1080i |
| 720p | 720p or 720i |

### Visual (`gv`)

HDR and video format tags. Each filter excludes higher-priority formats to avoid double-tagging.

| Filter | Matches |
|--------|---------|
| HDR10+ | HDR10+ / HDR10Plus / HDR10p, no DV |
| HDR10 | HDR10, no DV, no HDR10+ |
| HDR | HDR (generic), no DV, no HDR10 |
| IMAX Enhanced | IMAX Enhanced specifically |
| IMAX | IMAX without "enhanced" |
| DV | Dolby Vision with no audio codec match (standalone DV badge) |

### Audio (`ga`)

Audio codec tags. Each filter excludes higher-tier codecs so only the best matching codec badge is shown.

| Filter | Matches |
|--------|---------|
| DTS:X | DTS:X |
| DTS-HD MA | DTS-HD MA, no DTS:X |
| DTS-HD | DTS-HD, no MA, no DTS:X |
| DTS | DTS only, no HD/MA/X variants |
| Atmos+DV | Atmos + Dolby Vision (combined badge) |
| Atmos | Atmos, no DV |
| TrueHD+DV | TrueHD + Dolby Vision, no Atmos (combined badge) |
| TrueHD | TrueHD, no Atmos, no DV |
| DD++DV | Dolby Digital Plus + Dolby Vision, no Atmos/TrueHD (combined badge) |
| DD+ | Dolby Digital Plus (EAC3/DDP), no Atmos/TrueHD/DV |
| DD+DV | Dolby Digital + Dolby Vision, no DD+/Atmos/TrueHD (combined badge) |
| DD | Dolby Digital (AC3), no DD+/Atmos/TrueHD/DV |

### Channels (`gc`)

| Filter | Matches |
|--------|---------|
| 7.1 | 7.x or 8.x channel layout |
| 5.1 | 5.x channel layout, not 7.1 |

### Language (`gl`)

Text-only filters (no icon) using emoji flags as the tag name. Match language keywords and ISO codes in the stream name.

| Filter | Matches |
|--------|---------|
| 🇬🇧 | english / eng |
| 🇪🇸 | spanish / spa |
| 🇫🇷 | french / fra / fr / vff / vfq |
| 🇩🇪 | german / deu |
| 🇮🇹 | italian / ita |
| 🇵🇹 | portuguese / por |
| 🇯🇵 | japanese / jpn / Japanese script |
| 🇰🇷 | korean / kor / Korean script |
| 🇨🇳 | chinese / chi / Chinese script |
| 🇮🇳 | hindi / hin / Devanagari script |
| 🇸🇦 | arabic / ara / Arabic script |
| 🇷🇺 | russian / rus / Cyrillic script |
| 🌐 | multi / dual audio |
| 🇸🇰 | slovak / slk / slo / slovensky |
| 🇨🇿 | czech / cze / ces / cesky |
