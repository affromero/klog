# Changelog

All notable changes to klog are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] — 2026-05-15

First release under the new name `klog`. Previously published as `difflogtest`.

### Changed

- **Renamed package** from `difflogtest` to `klog`. The old name described a snapshot-test framework that is no longer part of this package.
- **Flattened the module layout.** The old `difflogtest.logging.*` and `difflogtest.utils.*` sub-packages have been merged into top-level modules:
  - `difflogtest.logging.core` → `klog.logger`
  - `difflogtest.logging.cache_tools` → `klog.cache`
  - `difflogtest.utils.strings` → `klog.time`
  - `difflogtest.utils.path` → `klog.path/` (split into `ops`, `io`, `query`, `env`)
- `logger.info_json` now uses stdlib `json` instead of `json5` — Pydantic-emitted JSON doesn't need comment tolerance.

### Removed

- **Snapshot test framework** (`UnitTests`, `@register_unittest`, `LogReplacement`, `is_unittest_mode`, `run-unittests` CLI). Not maintained; rely on `pytest` directly.
- **`seed_everything`** — pixelcache ships its own; this one only existed for the snapshot framework.
- **`wait_seconds_bar`** — zero consumers.
- **`path_download_and_extract_tar`** — drops the `wget` dependency.
- **`is_file_changed`, `logfile_from_func`, `path_from_pattern`, `keep_local_data`** — snapshot-only path helpers.

### Dropped dependencies

- `tyro` (snapshot CLI)
- `pytz` (unused)
- `json5` (info_json rewritten to stdlib json)
- `torch`, `torchvision` (seed_everything was the only consumer)
- `wget` (download helper removed)
- `GitPython` (`is_file_changed` was the only consumer)

### Kept

- `get_logger`, `LoggingRich`, `LoggingTable`, `DEFAULT_VERBOSITY`
- `lru_cache`, `DisableableLRUCache`, `get_cache_dir`, `sha256sum`
- All path helpers consumed by Hax-CV (`path_join`, `path_exists`, `path_mkdir`, `path_open`, `path_glob`, `path_basename`, `path_dirname`, `path_stem`, `path_is_s3`, `path_absolute`, `path_abs`, `path_dotenv`, `path_home`, `path_expanduser`, `path_relative_to`, `path_resolve`, `path_replace_suffix`, `path_rstrip`, `path_write_text`, `path_read_text`, `path_listdir`, `path_remove`, `path_remove_dir`, `path_copy`, `path_copy_dir`, `path_move`, `path_rename`, `path_symlink`, `path_islink`, `path_lexists`, `path_is_dir`, `path_is_file`, `path_is_image_file`, `path_is_video_file`, `path_getmtime`, `path_stat`, `path_dir_empty`, `path_exists_and_not_empty`, `path_startswith`, `path_endswith`, `path_newest_dir`, `path_newest_file`, `path_rglob`, `path_s3_bucket_name`, `is_glob_pattern`, `expand_glob_to_temp_dir`)
- `get_elapsed_time`

### Migration

Replace every `from difflogtest...` import:

| Old | New |
|-----|-----|
| `from difflogtest import get_logger` | `from klog import get_logger` |
| `from difflogtest import lru_cache, sha256sum` | `from klog import lru_cache, sha256sum` |
| `from difflogtest.utils.path import path_X` | `from klog.path import path_X` |
| `from difflogtest.logging.core import LoggingTable` | `from klog import LoggingTable` |

If you depended on `@register_unittest`, `is_unittest_mode`, or `LogReplacement` — those are gone. Use `pytest` directly.
