Intrinsic Images in the Wild
============================

This repository contains the decomposition algorithm presented in the paper:

	Sean Bell, Kavita Bala, Noah Snavely. "Intrinsic Images in the Wild".
	ACM Transactions on Graphics (SIGGRAPH 2014).
	http://intrinsic.cs.cornell.edu.'

If you find our code useful, please cite our paper.

The dataset is hosted at http://intrinsic.cs.cornell.edu/.


Dependencies
------------

- Eigen (http://eigen.tuxfamily.org/)
  On Ubuntu, you can install with: `sudo apt-get install libeigen3-dev`

- Python 2.7

- Python packages (newer packages will likely work, though these are the exact
  versions that I used):

      PIL==1.1.7
      cython==0.19.2
      numpy==1.8.0
      scipy==0.13.2
      scikit-image==0.9.3
      scikit-learn==0.14.1


Compile
-------

If on Ubuntu and you have installed Eigen3 to its default directory (/usr/include/eigen3),
then you can build the C++ extension with:

    cd krahenbuhl2013/
    make

If Eigen3 is in another directory, edit krahenbuhl2013/setup.py to change the directory.


Running
-------

	usage: decompose.py [-h] [-r <file>] [-s <file>] [-m <file>] [-j <file>]
						[-p <file>] [-l] [-q] [--show-labels]
						<file>

	Decompose an image using the algorithm presented in: Sean Bell, Kavita Bala,
	Noah Snavely. "Intrinsic Images in the Wild". ACM Transactions on Graphics
	(SIGGRAPH 2014). http://intrinsic.cs.cornell.edu. The output is rescaled for
	viewing and encoded as sRGB PNG images.

	positional arguments:
	<file>                Input image

	optional arguments:
	-h, --help            show this help message and exit
	-r <file>, --reflectance <file>
							Reflectance layer output name (saved as sRGB image)
	-s <file>, --shading <file>
							Shading layer output name (saved as sRGB image)
	-m <file>, --mask <file>
							Mask filename
	-j <file>, --judgements <file>
							Judgements file from the Intrinsic Images in the Wild
							dataset
	-p <file>, --parameters <file>
							Parameters file (JSON format). See bell2014/params.py
							for a list of parameters.
	-l, --linear          if specified, assume input is linear, otherwise assume
							sRGB
	-q, --quiet           if specified, don't print logging info
	--show-labels         if specified, also output labels


Embedding in other projects
---------------------------

Our code was written to be modular and can be embedded in larger projects.
The basic code for constructing the input and parameters can be found in
`bell2014/decompose.py` or is listed below:

    input = IntrinsicInput.from_file(
        image_filename,
        image_is_srgb=sRGB,
        mask_filename=mask_filename,
        judgements_filename=judgements_filename,
    )

	params = IntrinsicParameters.from_dict({
		'param1': ...,
		'param2': ...,
	})

    solver = IntrinsicSolver(input,params)
    r, s, decomposition = solver.solve()

    image_util.save(r_filename, r, mask_nz=input.mask_nz, rescale=True)
    image_util.save(s_filename, s, mask_nz=input.mask_nz, rescale=True)
