# Markdown Style

Keep Markdown easy to read, easy to diff, and lintable.

## Defaults

- Wrap prose at 100 characters where practical.
- Put a blank line after headings and between paragraphs.
- Put blank lines around lists, blockquotes, and fenced code blocks.
- Use fenced code blocks with triple backticks and specify the language.
- Prefer descriptive headings and consistent list punctuation.

## Code Blocks

- Use `bash` for shell commands, `rust` for Rust examples, `text` for literal output.
- Keep examples copy/pasteable; avoid ellipses in commands unless explicitly described.
- Show expected output or success criteria when it prevents ambiguity.

## Links

- Prefer link text that describes the destination ("Rustdoc conventions") over "here".
- Prefer relative links for repo-local docs.

## Linting

If `markdownlint-cli2` is available, run it and fix findings instead of silencing them.
