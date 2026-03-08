# Unofficial scikit-surprise Wheels for Windows (x64)

This repository provides **unofficial pre-built wheel files** (.whl) for the [scikit-surprise](https://github.com/NicolasHug/Surprise) Python package on **Windows 64-bit (amd64)**. scikit-surprise is a Python library for building and analyzing recommender systems, inspired by scikit-learn. The official package on PyPI only ships source distributions (no binaries), which requires compilation on Windows (needing Visual Studio Build Tools and handling Cython extensions). These wheels make installation easier without building from source.

These wheels are built from the latest source (v1.1.4 as of March 2026) and include patches for compatibility with newer Python and NumPy versions where needed. They are **not official** — use at your own risk. If you encounter issues, check the original repo's issues or contribute patches.

## Available Wheels
Wheels are available for the following Python versions (CPython, 64-bit):
- Python 3.11: `scikit_surprise-1.1.4-cp311-cp311-win_amd64.whl`
- Python 3.12: `scikit_surprise-1.1.4-cp312-cp312-win_amd64.whl`
- Python 3.13: `scikit_surprise-1.1.4-cp313-cp313-win_amd64.whl`
- Python 3.14: `scikit_surprise-1.1.4-cp314-cp314-win_amd64.whl`
- Python 3.15: `scikit_surprise-1.1.4-cp315-cp315-win_amd64.whl`

Download them from the [Releases](https://github.com/SaugatEDITH/surprise-wheels-windows/releases) page.

## Installation
1. Ensure you have the matching Python version installed (64-bit) from [python.org](https://www.python.org/downloads/).
2. Download the appropriate .whl file for your Python version.
3. Install via pip:
   ```
   pip install path/to/scikit_surprise-1.1.4-cp3XX-cp3XX-win_amd64.whl
   ```
   Replace `3XX` with your version (e.g., `cp312` for Python 3.12).

4. Test the installation:
   ```python
   import surprise
   print(surprise.__version__)  # Should print 1.1.4
   ```

## Compatibility Notes
- **NumPy Version**:
  - For wheels built for **Python 3.11 and 3.12**: These were compiled against NumPy <2.0 (e.g., 1.26.x). To avoid runtime errors (e.g., "numpy.ndarray size changed" or import crashes due to ABI incompatibility), install/use NumPy <2.0 in your environment:
    ```
    pip install "numpy<2.0"
    ```
    If you need NumPy 2.0+ for other dependencies, consider rebuilding the wheel yourself with patches (see below).
  - For wheels built for **Python 3.13, 3.14, and 3.15**: These were compiled against NumPy >2.0 (e.g., 2.4.x or later) with patches to fix deprecated types. They are compatible with NumPy 2.0+ and often work with older NumPy (1.x) too, but test in your setup. No downgrade needed — modern NumPy is recommended.

- **Platform**: Windows 10/11, 64-bit only. Built with Microsoft Visual Studio Build Tools 2022 (MSVC v143).
- **Dependencies**: Requires NumPy (as above). Other deps like scikit-learn are optional but recommended for full use.
- **Patches for 3.13+**: These wheels include manual patches to Cython files (e.g., replacing `np.int_t` with `cnp.int64_t`) to support NumPy 2.0+ and Python 3.13+ changes. See the [patch script](patch_surprise.py) in this repo for details.

## Usage Example
After installation:
```python
from surprise import SVD, Dataset, accuracy
from surprise.model_selection import train_test_split

# Load built-in dataset
data = Dataset.load_builtin('ml-100k')
trainset, testset = train_test_split(data, test_size=0.25)

# Train and predict
algo = SVD()
algo.fit(trainset)
predictions = algo.test(testset)

# Evaluate
print(accuracy.rmse(predictions))
```

For more, see the official [documentation](https://surprise.readthedocs.io/en/stable/).

## Building Your Own Wheels
If you need wheels for other versions or custom patches:
1. Install Visual Studio Build Tools (C++ workload).
2. Clone the original repo: `git clone https://github.com/NicolasHug/Surprise.git`.
3. For Python 3.13+: Run the [patch script](patch_surprise.py) provided here to fix NumPy types.
4. Install prereqs: `pip install --upgrade pip setuptools wheel cython numpy` (use <2.0 for 3.11/3.12, >2.0 for 3.13+).
5. Build: `python setup.py bdist_wheel` (in x64 Native Tools Command Prompt).
6. Wheel appears in `dist/`.

## Disclaimer
These are community-built wheels, not endorsed by the scikit-surprise maintainers. Test thoroughly in your environment. If issues arise with newer Python/NumPy, consider alternatives like LightFM or Implicit for more active maintenance.

## License
scikit-surprise is licensed under BSD-3-Clause. These wheels follow the same. See [LICENSE](LICENSE) for details.
