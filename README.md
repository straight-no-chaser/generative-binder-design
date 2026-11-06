# RFD-AF2-MPNN binder design and screening

A package for de novo protein binder design and large-scale screening combining RFdiffusion, ProteinMPNN, AlphaFold2 and Rosetta.

## Install

```bash
bash scripts/bootstrap_local.sh
```

## CLI Entry Points

Top-level project commands:

```bash
binderdesign init-project --help
binderdesign run-project --help
binderdesign submit-marlowe --help
```

Lower-level interfaces:

```bash
binderdesign table --help
binderdesign task --help
```

## Call

```bash
binderdesign run-project \
  --name demo \
  --target-pdb /path/to/target.pdb \
  --target-chain A \
  --binding-region A159,A161,A162,A163,A164,A165,A166,A167,A169 \
  --binder-length-min 60 \
  --binder-length-max 80 \
  --num-seeds 4 \
  --num-designs-per-seed 100 \
  --af-backend af2 \
  --submit-marlowe
```

![Example design of transducer of cdc42-dependent actin assembly](assets/example.png)

## Marlowe Environment Variables

### RFdiffusion

```bash
export RFDIFFUSION_PYTHON=/path/to/env/bin/python
export RFDIFFUSION_SCRIPT=/path/to/RFdiffusion/scripts/run_inference.py
```

Optional:

- `RFDIFFUSION_MODEL_DIR`
- `RFDIFFUSION_CKPT_PATH`
- `RFDIFFUSION_EXTRA_ARGS`
- `RFDIFFUSION_SIF`

### ProteinMPNN

```bash
export PROTEINMPNN_PYTHON=/path/to/env/bin/python
export PROTEINMPNN_SCRIPT=/path/to/ProteinMPNN/protein_mpnn_run.py
```

Optional:

- `MPNN_PDB_COLUMN`
- `MPNN_DESIGN_CHAINS`
- `MPNN_NUM_SEQ_PER_TARGET`
- `MPNN_SAMPLING_TEMP`
- `MPNN_BATCH_SIZE`
- `MPNN_EXTRA_ARGS`
- `PROTEINMPNN_SIF`

### AlphaFold2

```bash
export AF2_BATCH_CMD=colabfold_batch
```

Optional:

- `AF2_MODEL_TYPE`
- `AF2_EXTRA_ARGS`
- `AF2_SIF`
- `COLABFOLD_CMD`
- `COLABFOLD_MODEL_TYPE`
- `COLABFOLD_EXTRA_FLAGS`
- `COLABFOLD_SIF`

### Rosetta

```bash
export ROSETTA_BIN=/path/to/rosetta/main/source/bin
export ROSETTA_DB=/path/to/rosetta/main/database
```

Optional:

- `ROSETTA_INTERFACE`
- `ROSETTA_SIF`
- `ROSETTA_ROSETTASCRIPTS_BIN`
- `ROSETTA_FASTRELAX_BIN`
- `ROSETTA_RELAX_XML`
- `ROSETTA_DDG_BIN`
- `ROSETTA_DDG_EXTRA_ARGS`

## Notes

DDG supports a real Rosetta DDG wrapper through `ROSETTA_DDG_BIN`; if that is unset, falls back to InterfaceAnalyzer `dG_separated` as a coarse negative filter.