# ZebrafishXblastneuron
Testing [Blastneuron](https://pubmed.ncbi.nlm.nih.gov/26036213/) locally on 3000 zebrafish SWC neuron files.


# Scope
Use this pipeline to get interneuron similarity scores using their morphologies stored as SWC files. For more details about the theoretical basis for this project, consult the [project writeup](https://docs.google.com/document/d/19_llG6LcqTXlHoWjnk0PRMmGcrdg2sInRmTUwVZXbxA/edit?usp=sharing).


## Installation (getting BlastNeuron running)

Blastneuron, as it is presented in the original paper does not seem to work autonomously. However, it is available as a plugin of [Vaa3D](https://alleninstitute.org/what-we-do/brain-science/research/products-tools/vaa3d/).

How to get it working on your computer:
1. Download [version 3.200](https://github.com/Vaa3D/release/releases/)
2. Running the app:
- Windows: After downloading the zipped executable file, unzip and double click the exe file to install and then run.
- Mac: Download and unzip the file, then move it into /Applications folder, then you can find and right-click on the 'vaa3d.app' to launch it.
- Linux: After downloading Vaa3D, first run 'tar zxvf [filename]' and then enter the produced folder, and run './start_vaa3d.sh'.
3. In order to be able to compile plugins from Vaa3D (including blastneuron), follow the [instructions](https://github.com/Vaa3D/Vaa3D_Wiki/wiki/CompilePlugins.wiki) for your specific OS.
4. Get the source code for the plugins by running this in the terminal (for Mac):
```bash
git clone https://github.com/Vaa3D/v3d_external.git
git clone https://github.com/Vaa3D/vaa3d_tools.git
```
5. Start using BlastNeuron!


Tips for general [command line](https://github.com/Vaa3D/Vaa3D_Wiki/wiki/commandLineAccess.wiki) use of Vaa3D plugins.
You can also consult the official documentation about [Vaa3D](https://alleninstitute.org/what-we-do/brain-science/research/products-tools/vaa3d/), [plugins](https://github.com/Vaa3D/Vaa3D_Wiki/wiki/Vaa3DPlugins.wiki), or the [forum](https://www.nitrc.org/forum/message.php?msg_id=21874) for additional information/ instructions for Windows or Unix OS.

## Workflow

For this project, we used the following plugins and functions in the following order (we include the command line model code in parantheses - actual usage examples can be found in the python files of this project):

1. blastneuron - preprocessing (<vaa3d_executable> -x blastneuron -f pre_processing -p  "#i input.swc #o result.swc #s 2 #r 1 "
2. generating a linker file (<vaa3d_executable> -x linker_file -f linker -i input_folder -o mylinker.ano -p 1)
3. sorting the neuron SWCs - sort_swc (<vaa3d_executable> -x sort_neuron_swc -f sort_swc -i test.swc -o test_sorted.swc -p 0  1 )
4. blastneuron - batch computing features (<vaa3d_executable> -x blastneuron -f batch_compute -p "#i mydatabase.ano #o features.nfb")
5. blastneuron - global retrieving (<vaa3d_executable> -x blastneuron -f global_retrieve -p "#d features.nfb #q query.swc #n 10 #m 1,2,4 #o retrieve.ano")
6. blastneuron - inverse projection (<vaa3d_executable> -x blastneuron -f local_alignment -p "#t target.swc #s subject.swc #o match.swc")
7. blastneuron - local alignment (<vaa3d_executable> -x blastneuron -f inverse_projection -p "#i target.swc #o target_invp.swc")

A few considerations:
</br>
- <vaa3d_executable> must be replaced with the path to the executable of the vaa3d app. For Mac, it is `/Applications/Vaa3d_V3.20_MacOSX10.9_64bit/Vaa3d.app/Contents/MacOS/vaa3d`
- in order to get more details about a plugin (for example, blastneuron), just type `<vaa3d_executable> -x blastneuron -f help `
- to run these commands from python, you can use the **subprocess** library, for example 
```python
import subprocess

subprocess.run(<your_query>, shell = True, capture_output = True)
```

## Visualizations
During this project, it is often useful to be able to quickly visualize .swc files to check that the code is working as it should. For this, I used the python library [NAVIS](https://navis.readthedocs.io/en/latest/).

## Works to cite

Flease cite the following if using this plugin and/or software:

1.Wan Y, Long F, Qu L, Xiao H, Hawrylycz M, Myers EW, Peng H. BlastNeuron for Automated Comparison, Retrieval and Clustering of 3D Neuron Morphologies. Neuroinformatics. 2015 Oct;13(4):487-99. doi: 10.1007/s12021-015-9272-7. PMID: 26036213.

2.Peng, H., Ruan, Z., Long, F., Simpson, J.H., and Myers, E.W. (2010) "V3D enables real-time 3D visualization and quantitative analysis of large-scale biological image data sets," Nature Biotechnology, Vol. 28, No. 4, pp.348-353. (http://vaa3d.org)

3.Peng, H., Bria, A., Zhou, Z., Iannello, G., and Long, F. (2014) "Extensible visualization and analysis for multidimensional images using Vaa3D," Nature Protocols, Vol. 9, No. 1, pp. 193-208. (http://vaa3d.org)

4.Peng, H., et al. (2014) "Virtual finger boosts three-dimensional imaging and microsurgery as well as terabyte volume image visualization and analysis," Nature Communications, Vol. 5, No. 4342, DOI: 10.1038/ncomms5342.
Bria, A., et al. (2016) "TeraFly: real-time 3D visualization and 3D annotation of terabytes of multidimensional volumetric images," Nature Methods, Vol. 13, pp. 192-194, DOI: 10.1038/nmeth.3767.

5.Bria, A., et al. (2016) "TeraFly: real-time 3D visualization and 3D annotation of terabytes of multidimensional volumetric images," Nature Methods, Vol. 13, pp. 192-194, DOI: 10.1038/nmeth.3767.

