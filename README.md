**How to compute Drainage Lines with stream orders?**

**Step-1**: Start QGIS GUI, upload the shapefile and download SRTM data.

-   Go to Plugins \> SRTM-Downloader \> SRTM Downloader

-   Click on "Set canvas extent" , set the output path and click on
    > download.

![](.\Screenshots\srtm-downloader.png)

> \> Pop up window for login will appear to start download. Login using
> Earthdata
> credential.([[Register]{.underline}](https://urs.earthdata.nasa.gov//users/new)
> if new user)

**Step-2**: Compute the depression less dem using
[[this]{.underline}](https://colab.research.google.com/drive/1VNlfbskYnkDxM4mRN6IXGvSxygj9QpXr?usp=sharing)
python script.

Update these variables in the code(Line no. 26-28).

Shapefile_path: \>update your shapefile path.

Output_folder: \>update your output directory path.

Srtm_directory: \>update the path of the directory containing SRTM
tiles.

**Step-3**: Import the Depressionless dem tiles (.tif files) from the
output directory on qgis and build a virtual raster.

-   Go to Raster\> Miscellaneous \> Create Virtual Raster

-   Select all the .tif files in the Input layer

-   Untick the "Place each input file into separate band" and run

![](.\Screenshots\Build_Virtual_Raster.png)

**Step-4**: Clip the virtual raster using your shapefile as mask layer
and set the source and target crs=4326.

-   Go to Raster \> Extraction \> Clip raster by mask layer

-   Input layer : virtual raster

-   Mask layer: your shapefile

-   Source CRS: select "EPSG:4326"

-   Target CRS: select "EPSG:4326"

![](.\Screenshots\Clip_raster_by_Mask_Layer.png)

**Step-5**: Extract drainage lines by using "r.stream.extract" in QGIS,
set the flow accumulation threshold to 100 and geometry to line. Output
only the vector shapefile.

-   Go to Processing \> Toolbox \> Search "r.stream.extract"

-   Input Map(elevation map) : Clipped output

-   Min flow accumulation for streams: 100

![](.\Screenshots\r_stream_extract_1.png)

-   Advanced parameters \> output type : select "line"

-   Only Click Output "unique stream ids (vect)" and run.

![](.\Screenshots\r_stream_extract_2.png)

**Step-6**: Run the "Check validity" on QGIS to remove the improper
geometry in drainage lines.

-   Go to Processing \> Toolbox \> Search "Check Validity"

-   Input layer: unique stream ids result

-   Method : qgis

-   Only click on "valid output"

![](.\Screenshots\Check_validity.png)

-   Export the valid output as shapefiles.

**Step-7**. Assign stream order using
[[this]{.underline}](https://colab.research.google.com/drive/1OgUeJMww9lOYHDUGfu3FnCBJjkOTjQdH?usp=sharing)
python script.

Update these variables in the code.(Line 6 & 7)

-   Dl_dir: path of the directory containing drainage lines

-   So_dir: path to the output directory where drainage lines with
    > stream order will be stored.
