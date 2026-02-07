# Markdown Style

Keep Markdown easy to read, easy to diff, and lintable.

## Defaults

- Wrap prose at 100 characters unless the repo config sets a different limit.
- Put a blank line after headings and between paragraphs.
- Put a blank line before and after lists, blockquotes, and fenced code blocks.
- Use fenced code blocks with triple backticks and specify the language.
- Exception: in Rustdoc where Rust is implied, omit explicit code fence language if needed.
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
