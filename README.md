# 2D percolation

Python plotting notebooks and final results from a 2D square-lattice percolation study using the Newman–Ziff algorithm.

| Notebook | Results |
| --- | --- |
| [plotA](A_2DSpeed_DONE/plotA.ipynb) | Algorithm timings, fit, and residuals |
| [plotB](B_2Dpercolates/plotB.ipynb) | Percolation probabilities, threshold estimates, and finite-size scaling |
| [plotC](C_2Dprecise/plotC.ipynb) | Order parameter, cluster counts, and susceptibility |
| [plotD](D_2Dcorrlength_DONE/plotD.ipynb) | Correlation length, peak scaling, and data collapse |

## Run the notebooks

From the repository root, install the Python dependencies and fetch the [final data release](https://github.com/kevinshao7/percolation-2d/releases/tag/data-v1). The archive commands require GitHub CLI and a tar implementation with Zstandard support.

```sh
python -m pip install -r requirements.txt
gh release download data-v1 --dir archives --pattern B.tar.zst --pattern D-plot.tar.zst
tar -xf archives/B.tar.zst
tar -xf archives/D-plot.tar.zst
```

The timing notebook uses the CSV already in Git. The command above fetches the B and D archives. To run `plotC.ipynb`, download all three C parts and reassemble them from the repository root.

```sh
gh release download data-v1 --dir archives --pattern "C-plot.tar.zst.part*"
```

On macOS or Linux:

```sh
cat archives/C-plot.tar.zst.part{1,2,3} > archives/C-plot.tar.zst
tar -xf archives/C-plot.tar.zst
```

On Windows:

```powershell
cmd /c 'copy /b archives\C-plot.tar.zst.part1+archives\C-plot.tar.zst.part2+archives\C-plot.tar.zst.part3 archives\C-plot.tar.zst'
tar -xf archives/C-plot.tar.zst
```

Start JupyterLab with `jupyter lab` and open each notebook from its own folder. The archives restore the NumPy files to the relative paths the notebooks use. Allow about 20 GiB for the extracted data, downloaded archives, and reassembled C archive.

- A uses the committed timing summary CSV. Empty and unreadable batches were skipped, and the historical ten-slot normalization was retained.
- B contains the final probability means and uncertainties, stored as statistic × occupation count × direction (X, Y, either).
- C contains the C7 postprocessed means and uncertainties, stored as occupation count × quantity (order parameter, cluster count, susceptibility) × statistic. The broad occupation-probability plots use C7 because the older C6 postprocessed directory was unavailable. The susceptibility collapse uses the susceptibility column and rescales it by `L^(-gamma/nu)`.
- D contains the final correlation-length means and uncertainties, stored as occupation count × statistic. The archive contains the seven completed lattice sizes used by the notebook.
