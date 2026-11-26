# SIA harness documentation

`template_sia` demonstrates deterministic evaluation with a small threshold
classifier. The default pipeline replays recorded generations and builds a
paper from the resulting metrics. A local subprocess mode exercises the same
evaluation with the bundled reference classifier.

- [Setup, run, and verification](quickstart.md)
- [Architecture](architecture.md), [task layout and tokens](syntax_guide.md),
  and [test design](testing_philosophy.md)
- [Rendering pipeline](rendering_pipeline.md), [output inventory](output_conventions.md),
  and [troubleshooting](troubleshooting.md)
- [Manuscript](manuscript/README.md), [manuscript status](manuscript_status.md),
  and [publication configuration](publishing.md)
- [Project scripts](reference/scripts.md), [source modules](reference/source.md),
  [figures](reference/figures.md), [tests](reference/tests.md), and
  [claim ledger](reference/evidence.md)
- [Task](reference/mini_classify.md), [public data](reference/public_data.md),
  [held-out data](reference/private_data.md), and [reference classifier](reference/target_agent.md)
- [Forking](forking_guide.md), [FAQ](faq.md), [style guide](style_guide.md),
  [agent guidance](agent_instructions.md), and [backlog](TODO.md)
