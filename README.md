# Integrating-urinary-metabolomics-and-clinical-datasets-for-multi-cancer-detection

# LDA Metabolomics Analysis

A Python tool for performing Linear Discriminant Analysis (LDA) on metabolomics data with advanced visualization capabilities.

## Features

- **Multiple LDA projections** with flexible color mapping
- **Batch-aware analysis** supporting up to 31 unique batches
- **Disease classification** with customizable color schemes
- **Publication-ready outputs** in both SVG (editable) and PNG formats
- **Automatic legend management** for plots with many categories
- **Flexible data preprocessing** with multiple summarization modes

## Installation

### Prerequisites
- Python 3.10 or higher
- NumPy array files (.npy) containing metabolomics data
- Metadata CSV file with sample information

### Setup

1. Clone the repository:
```bash
git clone https://github.com/yourusername/lda-metabolomics.git
cd lda-metabolomics
```

2. Create a virtual environment (recommended):
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Data Structure

### Required Directory Structure
```
project_root/
├── data/
│   ├── metadata.csv
│   └── raw_files/
│       ├── sample1.npy
│       ├── sample2.npy
│       └── ...
├── output/  # Created automatically
├── config.yaml
└── run_analysis.py
```

### Metadata Format
Your `metadata.csv` must contain the following columns:
- `sample_barcode`: Unique identifier matching .npy filenames
- `batch_number`: Batch identifier (integer or string)
- `group_class`: Classification group
- `disease_name`: Disease category

Example:
```csv
sample_barcode,batch_number,group_class,disease_name
sample001,1,control,normal
sample002,1,treatment,diabetes
...
```

## Usage

### Basic Usage

1. Configure your analysis by editing `config.yaml`:
```yaml
paths:
  metadata: "data/metadata.csv"
  npy_dir: "data/raw_files"
  output_dir: "output"

analysis:
  summarize_mode: "mean"  # Options: mean, sum, flatten
  n_components: 2
  filter_diseases:  # Optional: specify diseases to analyze
    - normal
    - diabetes
    - hypertension
    - "diabetes + hypertension"
```

2. Run the analysis:
```bash
python run_analysis.py
```

### Advanced Usage

For custom analyses, you can use the package as a library:

```python
from lda_metabolomics import LDAAnalyzer, Config

# Load configuration
config = Config.from_yaml("config.yaml")

# Initialize analyzer
analyzer = LDAAnalyzer(config)

# Run analysis
analyzer.run_full_analysis()

# Or run specific analyses
analyzer.run_lda_by_label("disease_name")
analyzer.run_lda_with_custom_colors(
    lda_by="batch_number",
    color_by="disease_name"
)
```

## Outputs

The analysis generates the following plots:

1. **Standard LDA plots**:
   - LDA by batch_number
   - LDA by group_class
   - LDA by disease_name

2. **Cross-colored LDA plots**:
   - LDA by batch_number, colored by group_class
   - LDA by batch_number, colored by disease_name
   - LDA by disease_name, colored by batch_number

3. **Filtered analyses** (if configured):
   - LDA for specific disease subsets

All plots are saved in both:
- **SVG format**: Vector graphics with editable text (for publication)
- **PNG format**: High-resolution raster images (300 DPI)

## Customization

### Color Schemes

Default color palettes:
- `batch_number`: 31-color Husl palette
- `group_class`: Set1 palette (9 colors)
- `disease_name`: Tab10/Tab20 palette

To use custom colors, modify the `custom_colors` section in `config.yaml`:

```yaml
custom_colors:
  disease_name:
    normal: "#3b75ae"
    diabetes: "#83584e"
    hypertension: "#8c69b7"
    "diabetes + hypertension": "#7f7f7f"
```

### Plot Settings

Adjust plot appearance in `config.yaml`:

```yaml
plot_settings:
  figure_size: [10, 6]
  scatter_size: 250
  scatter_alpha: 0.9
  dpi: 300
  font_size: 10
  font_family: "Arial"
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Citation

If you use this tool in your research, please cite:

```bibtex
@software{lda_metabolomics,
  title = {LDA Metabolomics Analysis Tool},
  author = {Your Name},
  year = {2024},
  url = {https://github.com/yourusername/lda-metabolomics}
}
```

## Acknowledgments

- Original implementation: ChatGPT (o3) - 2025-08-08
- Enhanced for GitHub sharing with modular architecture and flexible configuration
