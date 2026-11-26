# Install skills

IMPORTANT: Don’t commit the symlinks; they point to your local checkout.

From this repo’s root, set `target_dir` and run the block to link skills into the target project’s `.agents/skills/` directory:

```bash
target_dir="/absolute/path/to/project"

skills=("$PWD"/skills/*)

(
  cd "$target_dir" || exit 1
  install_dir=".agents/skills"
  mkdir -p "$install_dir" || exit 1
  failed=0
  for skill in "${skills[@]}"; do
    if [ -z "$skill" ] || [ ! -f "$skill/SKILL.md" ]; then
      printf 'Invalid skill directory: %s\n' "${skill:-<empty>}" >&2
      failed=1
      continue
    fi
    dest="$install_dir/${skill##*/}"
    [ -e "$dest" ] || [ -L "$dest" ] || ln -s "$skill" "$dest" || failed=1
  done
  exit "$failed"
)
```

Optional: to link only specific skills, replace the `skills=` line before running the block, for example:

```bash
skills=("$PWD/skills/astro-action-composition" "$PWD/skills/test-composition")
```

Existing paths are left untouched. Restart your agent in the target project to load the skills. In Pi, you can run `/reload` instead.
