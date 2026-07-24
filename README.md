# MLOps

A minimal MLOps pipeline that trains a Logistic Regression classifier on the Iris dataset, saves the model and metrics as artifacts, and runs the full training workflow through GitHub Actions CI.

---

## Project Structure

```
.
├── .github/
│   └── workflows/
│       └── ci.yml          # GitHub Actions pipeline
├── artifacts/
│   ├── model.pkl           # Serialized trained model
│   └── metrics.json        # Test accuracy after training
├── train.py                # Trains the model and writes artifacts
├── run.py                  # Loads the model and runs inference
├── requirements.txt        # Python dependencies
└── README.md
```

---

## Requirements

- Python 3.12 or 3.13
- pip

Install dependencies:

```bash
pip install -r requirements.txt
```

Dependencies:

| Package       | Version  |
|---------------|----------|
| scikit-learn  | 1.5.2    |
| joblib        | 1.4.2    |
| numpy         | 1.26.4   |

---

## Usage

### Train the model

Trains a Logistic Regression model on the Iris dataset, then saves the model and accuracy metrics under `artifacts/`.

```bash
python train.py
```

Output:

```
Saved model to artifacts/model.pkl
Test accuracy: 1.0000
```

### Run inference

Loads the saved model and predicts the class for a given feature vector passed as a JSON array.

```bash
python run.py --input "[5.1, 3.5, 1.4, 0.2]"
```

Output:

```
predictions: [0]
```

The `--input` argument must be a valid JSON array with four numeric values corresponding to the Iris feature set: sepal length, sepal width, petal length, petal width.

---

## CI Pipeline

The GitHub Actions workflow (`.github/workflows/ci.yml`) runs on every push and pull request to `main`. It executes the following steps across Python 3.12 and 3.13 in parallel:

1. Check out the repository
2. Set up the specified Python version
3. Upgrade pip, setuptools, and wheel
4. Install dependencies from `requirements.txt`
5. Run `train.py` and verify artifacts are created
6. Upload `artifacts/` as a versioned GitHub Actions artifact

Artifacts are named using the pattern `ml-artifacts-<python-version>-<run-id>`.

---

## Model Details

| Property   | Value                           |
|------------|---------------------------------|
| Algorithm  | Logistic Regression             |
| Dataset    | Iris (sklearn built-in)         |
| Train split| 80%                             |
| Test split | 20%                             |
| max_iter   | 200                             |
| Test accuracy | 1.0                          |
