# Contributing to the Fail2Drive Leaderboard

> By submitting to the leaderboard you confirm that your model complies with the [Fail2Drive benchmark rules](https://github.com/autonomousvision/fail2drive#fail2drive-rules): no training on Fail2Drive scenarios; external pretraining is allowed.

## 1. Evaluate

Evaluate your model on the full benchmark using [`slurm_evaluate.py`](https://github.com/autonomousvision/fail2drive/blob/main/slurm_evaluate.py) with the default seeds `1 2 3`:

```bash
python slurm_evaluate.py --agent_file <agent> --agent_config <config> --out_root results/MyMethod
```

The script assigns each route a deterministic traffic manager seed (`route_id % 1000 + 10000 * seed`), which keeps paired Base/Generalization routes comparable across runs. Do not override this logic.

## 2. Compute your score

```bash
python tools/f2d_result_parser.py results/MyMethod --method MyMethod
```

The parser prints a LaTeX row ready to paste into the leaderboard.

> Make sure each seed contains all 200 result JSONs (600 total).

## 3. Open a pull request

Organize your evaluation results into `results/MyMethod/` with one subdirectory per seed (`1/`, `2/`, `3/`), each containing the 200 result JSONs for that seed. Then open a PR including:

- [ ] Your `results/MyMethod/` folder
- [ ] A new row in `f2d_leaderboard.tex` (use the LaTeX row printed by the parser); place it in the section matching your sensor setup (camera / LiDAR-fusion / privileged)
- [ ] A BibTeX entry in `references.bib`, with a key matching the `\cite{...}` in your new row

  Match the style already used in the file: fields aligned on `=`, ordered `author, title, booktitle/journal, ..., year`, and a `AuthorYEARTitle` key (e.g. `Smith2026MyMethod`). Use `@inproceedings` for published work, or `@misc` for arXiv-only preprints:

  ```bibtex
  @inproceedings{Smith2026MyMethod,
    author    = {Smith, Jane and Doe, John},
    title     = {MyMethod: A Great Driving Policy},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    year      = {2026},
    pages     = {1-10}
  }

  @misc{Smith2026MyMethod,
    author        = {Smith, Jane and Doe, John},
    title         = {MyMethod: A Great Driving Policy},
    year          = {2026},
    eprint        = {2601.00000},
    archivePrefix = {arXiv},
    primaryClass  = {cs.RO},
    url           = {https://arxiv.org/abs/2601.00000}
  }
  ```

In the pull request, please provide some information about your model and a link to your paper. We would also appreciate a short note on where your model succeeded and failed, including which scenario categories caused the largest generalization drops, and any notable failure patterns.

## Questions?

Open an issue in the [main repo](https://github.com/autonomousvision/fail2drive/issues) or join our [Discord](https://discord.gg/HZ83Em6kyZ).
