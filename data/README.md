# Data

This project uses the **Rossmann Store Sales** dataset, not stored here because
it is distributed under the Kaggle competition rules.

## Download

1. Go to https://www.kaggle.com/c/rossmann-store-sales/data
2. Download and unzip.
3. Place `train.csv` and `store.csv` in the same folder as the `.Rmd` files
   (i.e. in `code/`, or adjust `DATA_DIR` at the top of each `.Rmd`).

Or with the Kaggle CLI:

```bash
kaggle competitions download -c rossmann-store-sales
unzip rossmann-store-sales.zip
```

Required files: `train.csv`, `store.csv`.
