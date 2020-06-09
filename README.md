# NASA-IMPACT datashare program



## Phenomena

### Tropical Cyclone
Tropical cyclones are one of the most impactful meteorological phenomena observed on Earth. Tropical cyclones are defined as intense, synoptic scale storms, originating over warm tropical waters characterized by low pressure, high winds, and heavy precipitation. The combination of impacts from heavy wind and precipitation can cause widespread damage and flooding resulting in billions of U.S. dollars of assistance for impacted regions.

![Tropical Cyclone Example]  (/examples/hurricane.jpg)

Data Description:
1. 202 positive images are stored in a folder with the following structure:

### Smoke
Smoke is a mixture of gases and particles released in response to the combustion of living or dead biomass. Anthropogenic combustion emissions (such as from power plants or from fossil fuel rig flaring) may also be considered smoke. The exact combustion of smoke is highly dependent upon the fuel, ambient atmospheric chemical composition and meteorological (or physical) conditions.

![Smoke Example]  (/examples/smoke.jpg)
1. 256 positive images are stored in a folder with the following structure:

### Dust
Dust is composed of fine particles of solid matter originating from Earth's surface. Sources of dust include soil, sand, volanic eruptions, and pollution among others. For dust events to be detected using satellite imagery, dry and windy conditions are typically required within the atmospheric boundary layer.

![Dust Example]  (/examples/dust.jpg)
1. 200 positive images are stored in a folder with the following structure:

### Transverse Cirrus Bands
Transverse Cirrus bands are irregularly spaced bandlike cirrus clouds that form nearly perpendiular to the axis of maximum wind speed in a jet stream. While the cause of their formation is currently unknown, transverse cirrus bands tend to be collocated with atmospheric phenomena that are known to exhibit vertical and horizontal wind shear. Thus, transverse cirrus bands can identify regions where flight-level winds may be turbulent for air travel.

![Transverse Cirrus Band Example]  (/examples/transverse_band.jpg)
1. x positive images are stored in a folder with the following structure

### High Latitude Dust
Dust  aerosols  in  the  atmosphere  are  known  to  modulate environmental conditions and the climate system through direct and indirect effects between the land–atmosphere–ocean system. High latitude dust (HLD) is defined as ”particles that are lifted from a surface and travel by suspension in the atmosphere”. Typically, these dust particles are of the range of tenths of μm or larger in size. In addition,  dust events are considered high latitude when they are ≥ 50◦N and ≥ 40◦S. HLD  tends  to  occur  in  specific conditions and is seasonally dependent. HLD events can last anywhere from several hours to several days.

![High Latitude Dust Sample](/examples/hld_sample.jpg)

Data Description:
1. Labeled events are stored in a folder with the following structure:
  ```
   high-latitude-dust_<date>_<id>:
     |> high-latitude-dust_<date>_<id>.dbf
     |> high-latitude-dust_<date>_<id>.prj
     |> high-latitude-dust_<date>_<id>.shp
     |> high-latitude-dust_<date>_<id>.shx
  ```
2. Images to be downloaded using GIBS. (https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi?SERVICE=WMS&REQUEST=GetMap&layers=MODIS_Aqua_CorrectedReflectance_TrueColor&version=1.3.0&crs=EPSG:4326&transparent=false&width={}&height={}&bbox={}&format=image/tiff&time={})
2.a. Variables required:
     > width = width of the image
     > height = height of the image
     > bbox = [left_latitude, left_longitude, right_latitude, right_longitude]

*Note: Use the following for approximate width, height calculation and url generation.*

```
URL = "https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi?SERVICE=WMS&REQUEST=GetMap&layers=MODIS_Aqua_CorrectedReflectance_TrueColor&version=1.3.0&crs=EPSG:4326&transparent=false&width={}&height={}&bbox={}&format=image/tiff&time={}"
KM_PER_DEG_AT_EQ = 111.


def calculate_width_height(extent, resolution):
    lats = extent[::2]
    lons = extent[1::2]
    km_per_deg_at_lat = KM_PER_DEG_AT_EQ * np.cos(np.pi * np.mean(lats) / 180.)
    width = int((lons[1] - lons[0]) * km_per_deg_at_lat / resolution)
    height = int((lats[1] - lats[0]) * KM_PER_DEG_AT_EQ / resolution)
    print(width, height)
    return (width, height)


def modis_url(time, extent, resolution):
    width, height = calculate_width_height(extent, resolution)
    extent = ','.join(map(lambda x: str(x), extent))
    return (width, height, URL.format(width, height, extent, time))
```

*You can find the example notebook [here](/examples/url_generator.ipynb)*

You can download the files from: `s3://hld-datashare`. You will need an AWS account to download the data.

### Cloud Streets

The organization of cumulus clouds into elongated rows oriented parallel to the mean boundary layer flow is a phenomenon often referred to as cloud streets. Organized cumulus cloud streets are the visual manifestation of underlying boundary layer roll circulations commonly referred to as horizontal convective rolls. Formation of boundary layer rolls is attributed to  two instability mechanisms – thermal and dynamic instability.

Direct impacts of cloud streets are fairly minimal as they typically do not precipitate or have meaningful environmental impacts aside from surface radiation.  The effects of cloud streets are most notable in the presence of sea breeze circulations as intersections of roll circulations and sea breeze circulations are known to force convective initiation.  Coastal convection is a primary source of precipitation in these settings and can grow upscale to propagating mesoscale convective systems that modulate the regional and even global precipitation budgets.

![Cloud Street Sample](/examples/cloudstreet_sample.jpg)

1. Folder Structure:
  ```
   Cloud Streets/
       |> yes
          |> <images>
       |> no
          |> <images>
  ```
2. Data type: images, jpg

You can download the files from: `s3://cloudstreet-datashare`. You will need an AWS account to download the data.

### Additional Images
The development of machine learning models requires additional data with negative images (i.e. images that do not contain the phenomena of interest) that are used for training the model. Within this data we include 334 negative images that can be used interchangably between the models as null images for training.
