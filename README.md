# ppi disjoint

A small, honest protein protein interaction (PPI) classifier that runs in Google Colab.

Give it two human protein sequences and it estimates how likely they are to interact. It uses a frozen ESM2 protein language model (the 35M parameter version from Meta) to turn each sequence into an embedding, then a small neural network on top to decide whether the pair interacts.

## Why this exists

Most PPI benchmarks split the data by pairs. That means the same protein can show up in a training pair and again in a test pair, so a model can score well just by remembering proteins it has already seen. This notebook splits by protein instead. Whole proteins are assigned to train, validation or test first, and only then are pairs formed. Pairs whose two proteins land in different groups are thrown away. A built in check confirms that zero proteins are shared between train and test.

The goal is a number that reflects generalization to proteins the model has never seen, even if that number is lower.

## Results so far

On the human benchmark data from D-SCRIPT, with the protein disjoint split.

| Split | AUROC | AUPR |
| --- | --- | --- |
| Official pair split (earlier run) | 0.9586 | 0.796 |
| Protein disjoint split (this notebook) | 0.9137 | 0.6214 |

The drop is expected and confirms that part of the earlier score came from proteins repeated across train and test. At a 0.5 threshold the "interacts" class has precision 0.38 and recall 0.75, so this is useful for ranking candidate pairs, not as a final yes or no filter.

## Honest status

This is version 0.1 and the first public release. It works end to end, but it is early and rough, and more is coming.

Things to keep in mind right now.

1. This is a didactic replication with a frozen ESM2 and a simple classifier. It makes no claim of novelty.
2. Results come from a single run with one random seed, on a test set of 4637 pairs with only 326 positives, so expect some variance.
3. Negatives are assumed non interacting, as in the original benchmark, which is an assumption and not a ground truth.
4. The notebook text and comments are currently written in Portuguese. An English version is planned.
5. This is not a drug discovery tool. It only predicts protein protein interaction from sequence. It knows nothing about small molecules or binding affinity.

## Roadmap

The next versions are planned to include the following.

1. English notebook text and comments.
2. Saving the checkpoint and the embeddings to Google Drive so they survive a session reset.
3. Choosing the decision threshold on the validation set instead of fixing it at 0.5.
4. A simple function that takes two sequences and returns an interaction probability.
5. Fine tuning the last ESM2 layers and trying a larger ESM2 model.
6. Multiple seeds with confidence intervals.
7. Later, a separate notebook for molecule and protein binding affinity on public benchmarks.

## How to run

Open ppi_classifier_protein_disjoint.ipynb in Google Colab, choose a GPU runtime (a T4 is enough) and run the cells from top to bottom. The notebook downloads the data on its own, so nothing needs to be uploaded.

## Credits

The data comes from the D-SCRIPT project by Sledzieski, Singh, Cowen and Berger (Cell Systems, 2021). The protein language model is ESM2 from Meta AI (Lin et al., Science, 2023), loaded through Hugging Face Transformers. All credit for the dataset and the model belongs to their authors.

## License

MIT, see the LICENSE file. Use it, change it and build on it. Issues and pull requests are welcome.

## Author

Samuel Pedroso, Brazil. Building AI systems, applications and games.
