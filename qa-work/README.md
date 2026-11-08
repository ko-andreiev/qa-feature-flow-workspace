# qa-work/

QA artifacts live here. On the first run, setup writes the templates, the tag dictionary and the folders for feature
notes, test cases and run journals into this folder; every phase then adds its notes, test cases and run journals and
commits them at its gate. The exact layout comes from `feature-flow.toml` (section `[qa]`) at the workspace root,
which setup creates.
