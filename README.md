[![Estado de compilación](https://travis-ci.org/rordenlab/dcm2niix.svg?branch=master)](https://travis-ci.org/rordenlab/dcm2niix)
[![Estado de compilación](https://ci.appveyor.com/api/projects/status/7o0xp2fgbhadkgn1?svg=true)](https://ci.appveyor.com/project/neurolabusc/dcm2niix)

## Acerca de

dcm2niix está diseñado para convertir datos de neuroimagen del formato DICOM al formato NIfTI. Esta página web alberga el código fuente de desarrollo: se incluye una versión compilada para Linux, MacOS y Windows de la versión estable más reciente con [MRIcroGL] (https://www.nitrc.org/projects/mricrogl/). Un manual completo para este software está disponible en forma de [wiki de NITRC] (http://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage).

El formato DICOM es el formato de imagen estándar generado por los modernos dispositivos de imágenes médicas. Sin embargo, DICOM es muy [complicado](https://github.com/jonclayden/divest) y ha sido interpretado de manera diferente por diferentes proveedores. El formato NIfTI es popular entre los científicos, es muy simple y explícito. Sin embargo, esta simplicidad también impone limitaciones (por ejemplo, exige cortes equidistantes). dcm2niix también puede generar un [formato JSON de BIDS] (https://bids-specification.readthedocs.io/en/stable/) `sidecar` que incluye información relevante para los científicos del cerebro en un formato legible por humanos e independiente del proveedor.
El [Manual de neuroimágenes DICOM y NIfTI](https://github.com/DataCurationNetwork/data-primers/blob/master/Neuroimaging%20DICOM%20and%20NIfTI%20Data%20Curation%20Primer/neuroimaging-dicom-and-nifti-data -curation-primer.md) proporciona detalles.

## Licencia

Este software es de código abierto. La mayor parte del código está cubierto por la licencia BSD. Algunas unidades son de dominio público (nifti*.*, miniz.c) o utilizan la licencia MIT (ujpeg.cpp). Consulte el archivo license.txt para obtener más detalles.

## Dependencias

Este software debería ejecutarse en macOS, Linux y Windows normalmente sin necesidad de ningún otro software. Sin embargo, si usa dcm2niix para crear imágenes comprimidas con gz, será más rápido si tiene instalado [pigz](https://github.com/madler/pigz). Puede obtener una versión de dcm2niix y pigz compilada para su sistema operativo descargando [MRIcroGL](https://www.nitrc.org/projects/mricrogl/).

## Conversión y compresión de imágenes

DICOM proporciona muchas formas de almacenar/comprimir datos de imágenes, conocidas como [sintaxis de transferencia](https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage#DICOM_Transfer_Syntaxes_and_Compressed_Images). El [archivo COMPILE.md describe los detalles] (./COMPILE.md) sobre cómo habilitar diferentes opciones para brindar compatibilidad con más formatos.

- El código base incluye compatibilidad con la decodificación sin pérdidas JPEG sin formato, con codificación de longitud de ejecución y clásica.
- El JPEG con pérdida es manejado por el [NanoJPEG] incluido (https://keyj.emphy.de/nanojpeg/). Este soporte es modular: puede compilar para [libjpeg-turbo](https://github.com/chris-allan/libjpeg-turbo) o deshabilitarlo por completo.
- La compatibilidad con JPEG-LS sin pérdidas es opcional y se puede proporcionar mediante [CharLS](https://github.com/team-charls/charls).
- La compatibilidad con JPEG2000 con pérdida y sin pérdida es opcional y se puede proporcionar mediante [OpenJPEG](https://github.com/uclouvain/openjpeg) o [Jasper](https://www.ece.uvic.ca/~frodo/ jaspe/).
- La compresión GZ (por ejemplo, la creación de imágenes .nii.gz) es opcional y se puede proporcionar utilizando el [miniz] incluido (https://github.com/richgel999/miniz) o el popular zlib. Cabe destacar que [Cloudflare zlib](https://github.com/cloudflare/zlib) aprovecha el hardware moderno (disponible desde 2008) para una compresión muy rápida. Alternativamente, puede compilar dcm2niix sin un compresor gzip. Independientemente de cómo compile dcm2niix, puede usar el programa externo [pigz](https://github.com/madler/pigz) para la compresión paralela.

## Versiones

[Ver comunicados](https://github.com/rordenlab/dcm2niix/releases) para ver las notas de lanzamiento recientes. [Consulte el archivo VERSIONS.md para obtener detalles sobre versiones anteriores] (./VERSIONS.md).

## Contribuir

dcm2niix está desarrollado por la comunidad para la comunidad y todos pueden formar parte de la [comunidad] (./CONTRIBUTE.md).

## Correr

El uso de la línea de comandos se describe en el [wiki de NITRC] (https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage#General_Usage). La llamada de línea de comando mínima sería `dcm2niix /path/to/dicom/folder`. Sin embargo, es posible que desee invocar opciones adicionales, por ejemplo, la llamada `dcm2niix -zy -f %p_%t_%s -o /path/output /path/to/dicom/folder` guardará los datos como gzip comprimido, con el nombre de archivo basado en el nombre del protocolo (%p), tiempo de adquisición (%t) y número de serie DICOM (%s), con todos los archivos guardados en la carpeta "salida". Para obtener más ayuda, consulte la ayuda: `dcm2niix -h`.

[Consulte el archivo BATCH.md para obtener instrucciones sobre el uso de la versión de procesamiento por lotes] (./BATCH.md).

## Instalar

Hay un par de formas de instalar dcm2niix
- [Lanzamientos de Github](https://github.com/rordenlab/dcm2niix/releases) proporciona los ejecutables compilados más recientes. Esta es una excelente opción para usuarios de MacOS y Windows. Sin embargo, el ejecutable de Linux proporcionado requiere una versión reciente de Linux (por ejemplo, Ubuntu 14.04 o posterior), por lo que el ejecutable de Unix proporcionado no es adecuado para distribuciones muy antiguas. Específicamente, requiere Glibc 2.19 (a partir de 2014) o posterior. Los usuarios de sistemas más antiguos pueden compilar su propia copia de dcm2niix o descargar la versión compilada incluida con MRIcroGL Glibc 2.12 (desde 2011, ver más abajo).
- Ejecute el siguiente comando para obtener la última versión para Linux, Macintosh o Windows:
* `curl -fLO https://github.com/rordenlab/dcm2niix/releases/latest/download/dcm2niix_lnx.zip`
* `curl -fLO https://github.com/rordenlab/dcm2niix/releases/latest/download/dcm2niix_mac.zip`
* `curl -fLO https://github.com/rordenlab/dcm2niix/releases/latest/download/dcm2niix_mac_arm.pkg`
* `curl -fLO https://github.com/rordenlab/dcm2niix/releases/latest/download/dcm2niix_win.zip`
- [MRIcroGL (NITRC)](https://www.nitrc.org/projects/mricrogl) o [MRIcroGL (GitHub)](https://github.com/rordenlab/MRIcroGL12/releases) incluye dcm2niix que se puede ejecutar desde la línea de comando o desde la interfaz gráfica de usuario (seleccione el elemento de menú Importar). La versión de Linux de dcm2niix se compila en un [cuadro de compilación sagrado] (https://github.com/phusion/holy-build-box), por lo que debería ejecutarse en cualquier distribución de Linux.
- Si tiene una computadora MacOS con Homebrew o MacPorts, puede ejecutar `brew install dcm2niix` o `sudo port install dcm2niix`, respectivamente.
- Si tiene Conda, [`conda install -c conda-forge dcm2niix`](https://anaconda.org/conda-forge/dcm2niix) en Linux, MacOS o Windows.
- En computadoras Debian Linux puede ejecutar `sudo apt-get install dcm2niix`.


## Construir desde la fuente

A menudo es más fácil descargar e instalar una versión precompilada. Sin embargo, también puede compilar desde la fuente.

### Cree una versión de línea de comandos con cmake (Linux, MacOS, Windows)

`cmake` y `pkg-config` (opcional) se pueden instalar de la siguiente manera:

Ubuntu: `sudo apt-get install cmake pkg-config`

MacOS: `brew install cmake pkg-config` o `sudo port install cmake pkgconfig`

**Construcción básica:**
```bash
git clone https://github.com/rordenlab/dcm2niix.git
cd dcm2niix
mkdir build && cd build
cmake ..
make
```
Se creará `dcm2niix` en la subcarpeta `bin`. Para instalar en el sistema, ejecute `make install` en lugar de `make`; esto copiará el ejecutable en su ruta para que no tenga que proporcionar la ruta completa al ejecutable.

En casos excepcionales, si cmake falla con un mensaje como `"Generador: la ejecución de make falló"`, podría arreglarse con ``sudo ln -s `which make` /usr/bin/gmake``.

**Construcción avanzada:**

Como se indica en la sección `Soporte de compresión y conversión de imágenes`, el software proporciona muchos módulos opcionales con características mejoradas. Una opción común podría ser incluir soporte para JPEG2000, [JPEG-LS](https://github.com/team-charls/charls) (esta opción requiere un compilador c++14), además de usar el alto rendimiento Biblioteca zlib de Cloudflare (esta opción requiere una CPU construida después de 2008). Para construir con estas opciones, simplemente solicítelas al configurar cmake:

```bash
git clone https://github.com/rordenlab/dcm2niix.git
cd dcm2niix
mkdir build && cd build
cmake -DZLIB_IMPLEMENTATION=Cloudflare -DUSE_JPEGLS=ON -DUSE_OPENJPEG=ON ..
make
```

**versión de procesamiento por lotes opcional:**

El binario de procesamiento por lotes `dcm2niibatch` es opcional. Para compilar `dcm2niibatch` también cambie el comando cmake a `cmake -DBATCH_VERSION=ON ..`. Esto requiere un compilador que soporte c++11.

### Construyendo la versión de línea de comando sin cmake

Si tiene algún problema con el script de compilación cmake descrito anteriormente o desea personalizar el software, consulte el archivo [COMPILE.md para obtener detalles sobre la compilación manual] (./COMPILE.md).

## Referencias

- Li X, Morgan PS, Ashburner J, Smith J, Rorden C (2016) El primer paso para el análisis de datos de neuroimagen: conversión de DICOM a NIfTI. Métodos de J Neurosci. 264:47-56. doi: 10.1016/j.jneumeth.2016.03.001. [PMID: 26945974](https://www.ncbi.nlm.nih.gov/pubmed/26945974)

## Alternativas

- [BIDS-converter](https://github.com/openneuropet/BIDS-converter) aloja secuencias de comandos Matlab y Python para imágenes PET, compatibles con los formatos DICOM y ECAT (ecat2nii).
- [dcm2nii](https://people.cas.sc.edu/rorden/mricron/dcm2nii.html) es el predecesor de dcm2niix. Está obsoleto para las imágenes modernas, pero maneja formatos de imagen anteriores a DICOM (formatos propietarios de Elscint, GE y Siemens).
- Python [dcmstack](https://github.com/moloney/dcmstack) Conversión de DICOM a Nifti con conservación de metadatos.
- [dicm2nii](http://www.mathworks.com/matlabcentral/fileexchange/42997-dicom-to-nifti-converter) está escrito en Matlab. El lenguaje Matlab hace que esto sea muy programable.
- [dicom2nifti](https://github.com/icometrix/dicom2nifti) utiliza el envoltorio Python programable que utiliza los ejecutables [GDCMCONV de alto rendimiento](http://gdcm.sourceforge.net/wiki/index.php/Gdcmconv).
- [dicomtonifti](https://github.com/dgobbi/vtk-dicom/wiki/dicomtonifti) aprovecha [VTK](https://www.vtk.org/).
- [dimon](https://afni.nimh.nih.gov/pub/dist/doc/program_help/Dimon.html) y [to3d](https://afni.nimh.nih.gov/pub/dist/ doc/program_help/to3d.html) se incluyen con AFNI.
- [dinifti](http://as.nyu.edu/cbi/resources/Software/DINIfTI.html) se centra en la conversión de datos de Siemens.
- [DWIConvert](https://github.com/BRAINSia/BRAINSTools/tree/master/DWIConvert) convierte imágenes DICOM a formatos NRRD y NIfTI.
- [mcverter](http://lcni.uoregon.edu/%7Ejolinda/MRIConvert/) tiene un excelente soporte para varios proveedores.
- [mri_convert](https://surfer.nmr.mgh.harvard.edu/pub/docs/html/mri_convert.help.xml.html) es parte del popular paquete FreeSurfer. En mi experiencia limitada, esta herramienta funciona bien para los datos de GE y Siemens, pero falla con los conjuntos de datos 4D de Philips.
- [MRtrix mrconvert](http://mrtrix.readthedocs.io/en/latest/reference/commands/mrconvert.html) es un útil convertidor de imágenes de propósito general y maneja bien los datos DTI. Es una excelente herramienta para las modernas imágenes mejoradas de Philips.
- [nanconvert](https://github.com/spinicist/nanconvert) utiliza la biblioteca ITK para convertir DICOM de GE y Bruker patentado a formatos estándar como DICOM.
- [Visor PET CT](http://petctviewer.org/index.php/feature/results-exports/nifti-export) para [Fiji](https://fiji.sc) puede cargar imágenes DICOM y exportarlas como NIfTI .
- [Plastimatch](https://www.plastimatch.org/) es una navaja suiza: calcula el registro, el procesamiento de imágenes, las estadísticas y tiene un convertidor de formato de imagen básico que puede convertir algunas imágenes DICOM a NIfTI o NRRD.
- [Simple Dicom Reader 2 (Sdr2)](http://ogles.sourceforge.net/sdr2-doc/index.html) utiliza [dcmtk](https://dicom.offis.de/dcmtk.php.en) para leer imágenes DICOM y convertirlas al formato NIfTI.
- [Extensión SlicerHeart](https://github.com/SlicerHeart/SlicerHeart) está diseñada específicamente para ayudar a 3D Slicer a admitir imágenes de ultrasonido (EE. UU.) almacenadas como DICOM.
- [spec2nii](https://github.com/wexeee/spec2nii) convierte la espectroscopia de RM en NIFTI.
- [SPM12](http://www.fil.ion.ucl.ac.uk/spm/software/spm12/) es una de las herramientas más populares en el campo. Incluye conversión de DICOM a NIfTI. Al estar basado en Matlab, es fácil de programar.

## Enlaces

- [Tabla de convertidores DICOM a BIDS](https://bids.neuroimaging.io/benefits#mri-and-pet-converterss)

Las siguientes herramientas explotan dcm2niix

- [abcd-dicom2bids](https://github.com/DCAN-Labs/abcd-dicom2bids) descarga de forma selectiva conjuntos de datos ABCD de alta calidad.
- [autobids](https://github.com/khanlab/autobids) automatiza dcm2bids que usa dcm2niix.
- [BiDirect_BIDS_Converter](https://github.com/wulms/BiDirect_BIDS_Converter) para la conversión de DICOM al estándar BIDS.
- [BIDS Toolbox](https://github.com/cardiff-brain-research-imaging-centre/bids-toolbox) es un servicio web para la creación y manipulación de conjuntos de datos BIDS, utilizando dcm2niix para importar datos DICOM.
- [BIDScoin](https://github.com/Donders-Institute/bidscoin) es un convertidor de DICOM a BIDS con una GUI y [documentación] completa (https://bidscoin.readthedocs.io).
- [bidsconvertr](https://github.com/wulms/bidsconvertr) usa R para convertir datos DICOM a NIfTI y finalmente a BIDS.
- [bidsify](https://github.com/spinoza-rec/bidsify) es un proyecto de Python que utiliza dcm2niix para convertir imágenes DICOM y Philips PAR/REC al estándar BIDS.
- [bidskit](https://github.com/jmtyszka/bidskit) usa dcm2niix para crear [BIDS](http://bids.neuroimaging.io/) conjuntos de datos.
- [BioImage Suite Web Project](https://github.com/bioimagesuiteweb/bisweb) es un proyecto de JavaScript que utiliza dcm2niix para su módulo de conversión DICOM.
- [birc-bids](https://github.com/bircibrain/birc-bids) proporciona un contenedor Docker/Singularity con varias utilidades de conversión de BIDS.
- [BOLD5000_autoencoder](https://github.com/nmningmei/BOLD5000_autoencoder) usa dcm2niix para canalizar datos de imágenes en un algoritmo de aprendizaje automático no supervisado.
- [boutiques-dcm2niix](https://github.com/lalet/boutiques-dcm2niix) es un archivo docker para instalar y validar dcm2niix.
- [Brain imAgiNg Analysis iN Arcana (Banana)](https://pypi.org/project/banana/) es una colección de flujos de trabajo de análisis de imágenes cerebrales, utiliza dcm2niix para conversiones de formato.
- [brainnetome DiffusionKit](http://diffusion.brainnetome.org/en/latest/) utiliza dcm2niix para convertir imágenes.
- [BraTS-Preprocessor](https://neuronflow.github.io/BraTS-Preprocessor/) usa dcm2niix para importar archivos para [Brain Tumor Segmentation](https://www.frontiersin.org/articles/10.3389/fnins. 2020.00125/completo).
- [clinica](https://github.com/aramis-lab/clinica) es una plataforma de software para estudios clínicos de neuroimagen que utiliza dcm2niix para convertir imágenes DICOM.
- [clpipe](https://github.com/cohenlabUNC/clpipe) usa dcm2bids para importar DICOM.
- [conversion](https://github.com/pnlbwh/conversion) es una biblioteca de Python que puede convertir archivos NIfTI creados por dcm2niix al popular formato NRRD (incluidas las tablas de gradiente DWI). Tenga en cuenta que las versiones recientes de dcm2niix pueden convertir directamente imágenes DICOM a NRRD.
- [DAC2BIDS](https://github.com/dangom/dac2bids) utiliza dcm2niibatch para crear [BIDS](http://bids.neuroimaging.io/) conjuntos de datos.
- [Data2Bids](https://github.com/SIMEXP/Data2Bids) convierte imágenes que no son DICOM con archivos JSON asociados a BIDS. Si bien esta herramienta no requiere dcm2niix, puede aprovechar la salida de dcm2niix de manera similar a niix2bids.
- [Dcm2Bids](https://github.com/cbedetti/Dcm2Bids) usa dcm2niix para crear [BIDS](http://bids.neuroimaging.io/) conjuntos de datos. Aquí hay un [tutorial] (https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/BIDS_Overview.html) que describe el uso.
- [dcm2niir](https://github.com/muschellij2/dcm2niir) Envoltura R para dcm2niix/dcm2nii.
- [dcm2niix_afni](https://afni.nimh.nih.gov/pub/dist/doc/program_help/dcm2niix_afni.html) es una versión de dcm2niix incluida con [AFNI](https://afni.nimh.nih .gov/) distribución.
- [dcm2niiXL](https://github.com/neurolabusc/dcm2niiXL) es un script de shell y una compilación ajustada de dcm2niix diseñada para la conversión acelerada de conjuntos de datos extragrandes.
- [dcm2niixpy](https://github.com/Svdvoort/dcm2niixpy) Paquete Python de dcm2niix.
- [dcmwrangle](https://github.com/jbteves/dcmwrangle) una herramienta interactiva y estática de Python para organizar dicoms.
- [DeepDicomSort](https://github.com/Svdvoort/DeepDicomSort) puede reconocer diferentes tipos de escaneo.
- [DICOM-to-NIfTI-GUI](https://github.com/Zunairviqar/DICOM-to-NIfTI-GUI) es una secuencia de comandos de Python que proporciona un envoltorio gráfico para dcm2niix.
- [dicom2bids](https://github.com/Jolinda/lcnimodules) incluye módulos de python para convertir archivos dicom a nifti en una estructura de archivos compatible con bids que usa dcm2niix.
- [DICOM2BIDS](https://github.com/klsea/DICOM2BIDS) es un script de Python 2 para crear archivos BIDS.
- [dicom2nifti_batch](https://github.com/scanUCLA/dicom2nifti_batch) es un script de Matlab para automatizar dcm2niix.
- [desinvertir](https://github.com/jonclayden/divest) Interfaz R para dcm2niix.
- [ExploreASL](https://sites.google.com/view/exploreasl/exploreasl) usa dcm2niix para importar imágenes.
- [ezBIDS](https://github.com/brainlife/ezbids) es un [servicio web](https://brainlife.io/ezbids/) para convertir directorios llenos de imágenes DICOM en BIDS sin que los usuarios tengan que aprender Python ni archivo de configuración personalizado.
- [fmrif tools](https://github.com/nih-fmrif/fmrif_tools) utiliza dcm2niix para su herramienta [oxy2bids](https://fmrif-tools.readthedocs.io/en/latest/#).
- [fMRIprep.dcm2niix](https://github.com/BrettNordin/fMRIprep.dcm2niix) está diseñado para convertir el formato DICOM al formato NIfTI.
- [FreeSurfer](https://github.com/freesurfer/freesurfer) incluye dcm2niix para la conversión de imágenes.
- [fsleyes](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FSLeyes) es un potente visor de imágenes basado en Python. Utiliza dcm2niix para manejar archivos DICOM a través de sus bibliotecas fslpy.
- [Motor funcional de decodificación y neuromodulación endógena interactiva en tiempo real (FRIEND)] (https://github.com/InstitutoDOr/FriendENGINE) utiliza dcm2niix.
- [heudiconv](https://github.com/nipy/heudiconv) puede usar dcm2niix para crear [BIDS](http://bids.neuroimaging.io/) conjuntos de datos. Los datos adquiridos mediante la convención [reproin](https://github.com/ReproNim/reproin) se pueden convertir fácilmente a BIDS.
- [Extensión de salida de ofertas de Horos (Osirix)] (https://github.com/mslw/horos-bids-output) es un complemento de OsiriX / Horos que utiliza dcm2niix para crear una salida de ofertas.
- [kipettools](https://github.com/mathesong/kipettools) usa dcm2niix para cargar datos PET.
- [LEAD-DBS](http://www.lead-dbs.org/) usa dcm2niix para [importar DICOM](https://github.com/leaddbs/leaddbs/blob/master/ea_dicom_import.m).
- Las versiones [lin4neuro](http://www.lin4neuro.net/lin4neuro/18.04bionic/vm/) como la versión en inglés l4n-18.04.4-amd64-20200801-en.ova incluyen MRIcroGL y dcm2niix preinstalados. Esto permite que el usuario con VirtualBox o VMWarePlayer use estas herramientas (y muchas otras herramientas de neuroimagen) en una máquina virtual gráfica.
- [MRIcroGL](https://github.com/neurolabusc/MRIcroGL) está disponible para MacOS, Linux y Windows y proporciona una interfaz gráfica para dcm2niix. Puede obtener copias compiladas del [sitio web de MRIcroGL NITRC] (https://www.nitrc.org/projects/mricrogl/).
- [neuro_docker](https://github.com/Neurita/neuro_docker) incluye dcm2niix como parte de un Dockerfile único y estático.
- [NeuroDebian](http://neuro.debian.net/pkgs/dcm2niix.html) proporciona una versión actualizada de dcm2niix para sistemas basados en Debian.
- [neurodocker](https://github.com/kaczmarj/neurodocker) genera archivos Dockerfiles [personalizados](https://github.com/rordenlab/dcm2niix/issues/138) con versiones específicas del software de neuroimagen.
- [neurodocker](https://github.com/kaczmarj/neurodocker) incluye dcm2niix como un Dockerfile de instalación sencilla y mínima.
- [NeuroElf](http://neuroelf.net) puede usar dcm2niix para convertir imágenes DICOM.
- [Base de datos de neuroinformática (NiDB)] (https://github.com/gbook/nidb) está diseñada para almacenar, recuperar, analizar y compartir datos de neuroimagen. Utiliza dcm2niix para control de calidad de imágenes y manejo de algunos formatos.
- [NiftyPET](https://niftypet.readthedocs.io/en/latest/install.html) proporciona reconstrucción y análisis de imágenes PET y utiliza dcm2niix para manejar imágenes DICOM.
- [niix2bids](https://github.com/benoitberanger/niix2bids) intenta convertir automáticamente las imágenes de resonancia magnética de Siemens convertidas por dcm2niix a BIDS.
- [nipype](https://github.com/nipy/nipype) puede usar dcm2niix para convertir imágenes.
- [PET2BIDS](https://github.com/openneuropet/PET2BIDS) usa dcm2niix para imágenes DICOM.
- [py2bids](https://github.com/Jolinda/py2bids) dcm2niix dicom a contenedor de conversión de ofertas.
- [pyBIDSconv proporciona un formato gráfico para convertir imágenes DICOM al formato BIDS] (https://github.com/DrMichaelLindner/pyBIDSconv). Incluye heurísticas predeterminadas inteligentes para identificar escaneos de Siemens.
- [pydcm2niix es un módulo de Python para trabajar con dcm2niix](https://github.com/jstutters/pydcm2niix).
- [pydra-dcm2niix](https://github.com/nipype/pydra-dcm2niix) es una interfaz de tareas que contiene Pydra para dcm2niix.
- [qsm](https://github.com/CAIsr/qsm) Software de mapeo de susceptibilidad cuantitativa.
- [reproin](https://github.com/ReproNim/reproin) es una configuración para la generación automática de conjuntos de datos BIDS compartibles y controlados por versión a partir de escáneres MR.
- [Retina_OCT_dcm2nii](https://github.com/Choupan/Retina_OCT_dcm2nii) convierte datos de tomografía de coherencia óptica (OCT) a NIfTI.
- [sci-tran dcm2niix](https://github.com/scitran-apps/dcm2niix) Flywheel Gear (docker).
- [shimming-toolbox](https://github.com/shimming-toolbox/shimming-toolbox) habilitó el shimming estático y en tiempo real, usando dcm2niix para importar datos DICOM.
- [Extensión SlicerDcm2nii](https://github.com/Slicer/ExtensionsIndex/blob/master/SlicerDcm2nii.s4ext) es un método para importar datos DICOM a Slicer.
- [tar2bids](https://github.com/khanlab/tar2bids) convierte tarball(s) DICOM a BIDS usando heudiconv que invoca dcm2niix.
- [TORTOISE](https://tortoise.nibib.nih.gov) se usa para procesar datos de MRI de difusión y usa dcm2niix para importar imágenes DICOM.
  - [TractoR (Tractografía ­con R) utiliza dcm2niix para la conversión de imágenes](http://www.tractor-mri.org.uk/TractoR-and-DICOM).
- [XNAT2BIDS](https://github.com/kamillipi/2bids) es una canalización xnat simple para convertir escaneos DICOM a salida compatible con BIDS.
