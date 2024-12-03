# No Maintenance Notice

I am no longer maintaining this repository, as I have been dealing with a few factors that have stopped me from constantly updating this project; work, college, and mental health. I recommend you to check out skzk's [streetlevel](https://github.com/sk-zk/streetlevel), as it is more updated, and better optimized.

<div align="center">

[![LOGO](https://raw.githubusercontent.com/shmugoh/sv-dlp/master/docs/banner.png)](#readme)

[![DOWNLOAD_PYPI](https://img.shields.io/badge/-PyPi-blue.svg?style=for-the-badge&logo=pypi&label=Download)](https://pypi.org/project/sv-dlp/)
[![DOWNLOAD_BIN](https://img.shields.io/github/v/release/shmugoh/sv-dlp?logo=windowsterminal&color=blue&label=Download&style=for-the-badge)](https://github.com/shmugoh/sv-dlp/releases/tag/latest)

[![TEST_PANO](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/test_pano.yml?branch=master&label=Download%20Service&style=for-the-badge)](https://github.com/shmugoh/sv-dlp/actions)
[![TEST_LL](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/test_ll.yml?branch=master&label=Metadata%20Service&style=for-the-badge)](https://github.com/shmugoh/sv-dlp/actions)
[![TEST_URL](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/test_url.yml?branch=master&label=URL%20Short%20Service&style=for-the-badge)](https://github.com/shmugoh/sv-dlp/actions)

![APPLE](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/service_apple.yml?style=flat-square&logo=apple&label=Apple%20Service)
![GOOGLE](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/service_apple.yml?style=flat-square&logo=googlemaps&label=Google%20Service)
![YANDEX](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/service_yandex.yml?style=flat-square&logo=yandexcloud&logoColor=ff4a39&label=Yandex%20Service)
![BAIDU](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/service_baidu.yml?style=flat-square&logo=baidu&label=Baidu%20Service)
![BING](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/service_bing.yml?style=flat-square&logo=microsoftbing&label=Bing%20Status)
![NAVER](https://img.shields.io/github/actions/workflow/status/shmugoh/sv-dlp/service_naver.yml?style=flat-square&logo=naver&label=Naver%20Status)

</div>

---

sv-dlp is a powerful API Wrapper for Street View platforms, as it simplifies downloading panoramic images,
fetching metadata, and creating short URLs. sv-dlp offers extensive internal functionality, including metadata
extraction and panorama stitching. It's highly adaptable, allowing developers to integrate it into their
projects.

This project has been made for educational purposes and all content scraped from
is owned by their respective authors.

For detailed documentation and usage examples, please refer to the [official documentation](https://shmugo.co/sv-dlp).

## **Warning**

Please note that sv-dlp relies on reverse-engineered APIs, so its functionality may depend on API stability.
If you encounter issues, report them with the "Dead API" label for a prompt resolution. Additionally,
respect service limitations, and rest assured that we're committed to keeping sv-dlp up to date and running
smoothly for all supported platforms.

## **Features**

- Download Panoramas
  - A panorama ID, a coordinate or a short URL can be parsed
    - **[Google]** Radius can be parsed if input is coordinates
  - Is multithreaded; each panorama tile row is in different threads for faster downloads
  - Saves individual tiles if `--save-tiles` is parsed
  - Download from .csv/.json files
- Fully print metadata
  - Coords, Date, Panorama ID and Gen can be printed separately
- Coordinates and short URLs are automatically turned to Panorama IDs
- Short links with a Panorama ID or coordinates

## **Services**

<!-- TODO: Change Progress Formula -->

| Service | Progress | Notes                                                                                                                                                    |
| :------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Google  | 99%      | Zoom 5 only obtains a portion of the face.                                                                                                               |
| Baidu   | 99%      | Translation between BD09MC and WGS84 is innacurate.                                                                                                      |
| Naver   | 99%      | Short Link function does not use its appropiate API call.                                                                                                |
| Yandex  | 99%      | Short Link function does not use the `shortenPath` API call.                                                                                             |
| Apple   | 95%      | Misc features not supported; can only obtain metadata by coords - credit to [sk-zk & retroplasma](https://github.com/shmugoh/sv-dlp/blob/master/CREDITS) |
| Bing    | 95%      | Misc features not implemented; can only obtain metadata by coords - credit to [sk-zk](https://github.com/shmugoh/sv-dlp/blob/master/CREDITS)             |

## **Installation**

To install sv-dlp, download it via PIP, as long as Python >3.10 is installed

```bash
$ pip install sv_dlp
$ sv-dlp
```

## Manual

If you prefer using a binary instead, check out the binaries [here](https://github.com/shmugoh/sv-dlp/releases/latest)
and add them to your `PATH` folder. Check out this [Windows Guide](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/)
for more information if you're not familiar with it.

## Getting Started

sv_dlp's features are contained within a class for syntax convenience, so to begin
import the module and declare a variable of the class

```python
import sv_dlp
sv_dlp = sv_dlp.sv_dlp()
```

### Notes

- Whenever you call any function that is relating to downloading, or translating coordinates
  to Panorama ID, `self.get_metadata()` is called within that function, therefore it is
  allocated in `self.metadata`. If you plan on tinkering with the metadata,
  it is is recommended to call `self_get_metadata()` first
  before calling any other function for syntax readibility.
- Metadata is returned as a `MetadataStructure` object. If you wish to use
  a dictionary instead, consider using the `.dict()` method
- If you wish to call a function with a different service than google, please use
  `self.set_service()` first before doing anything - input **must be lowercase**

## Downloading

To download panorama directly from coordinates:

```python
pano_img = sv_dlp.download_panorama(lat=6.603079535799973, lng=-73.99819681137278)
sv_dlp.postdownload.save_panorama(pano_img, sv_dlp.metadata)
```

To download panorama directly from Panorama ID:

```python
pano_id = sv_dlp.get_pano_id(lat=6.603079535799973, lng=-73.99819681137278)

pano_img = sv_dlp.download_panorama(pano_id)
sv_dlp.postdownload.save_panorama(pano_img, sv_dlp.metadata)
```

To download panorama directly from metadata:

```python
metadata = sv_dlp.get_metadata(lat=6.603079535799973, lng=-73.99819681137278)

pano_img = sv_dlp.download_panorama(metadata.pano_id, zoom=max)
sv_dlp.postdownload.save_panorama(pano_img, metadata)
```

To download tiles individually:

```python
pano, tiles = sv_dlp.download_panorama("YV7i9WYmvPqT5nEtFLq3SA")
sv_dlp.postdownload.save_tiles(tiles, sv_dlp.metadata)
```

You can also pass a single variable to `sv_dlp.download_panorama`,
as postdownload will automatically determine
if it holds the panorama and tiles.

## Metadata Tinkering

To obtain older and linked panoramas from given location:

```python
metadata = sv_dlp.get_metadata(lat=6.603079535799973, lng=-73.99819681137278, get_linked_panos=True)

for pano in metadata.timeline:
    print(pano)
for pano in metadata.linked_panos:
    print(pano)
```

To obtain a panorama's date:

```python
metadata = sv_dlp.get_metadata(lat=6.603079535799973, lng=-73.99819681137278)

date = metadata.date
print(date)
```

## **FAQ**

## **Metadata is being returned as a `MetadataStrucutre` object. Any other way to access it?**

Yes! You can use the `.dict()` method to retrieve the metadata as a dictionary
instead.

## **Why aren't all my coordinates from .csv downloading?**

Depending on how close the given coordinates are, it'll download the same panorama ID
no matter the difference. It's recommended to use .json or .csv with panorama IDs instead.

## **Can X Service be added?**

Depending on how accesible it is scraping-wise, yes.
**Feel free to submit in an issue** about the specific service and I'll gladly see
what I can do about it. If you're a developer though, refer to the next question.

## **I'm a developer and I want to add X service or improve the code. Can I?**

Sure! You're welcome to submit in a pull request as long as its to improve
this program, such as improving performance, fixing a bug, adding a service, etc.
For those who want to add a service, check out [the documentation for it](https://shmugo.co/sv-dlp/latest/contributing_services/).

## **License**

[MIT](https://raw.githubusercontent.com/shmugoh/sv-dlp/master/LICENSE)
