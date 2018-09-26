---
layout: post
title: Command Line Parser for dotnet Gotchas

---
I am talking of course about the CommandLineParser Nuget library. Specifically, version 2.1.1 at the time of this blog post.

## Verb Commands

In order for the verb command functionality to be working with the fluent `MapResult` or `ParsedResult` methods, you need to have at least two options passed in. Otherwise, the `ParseArguments` will treat it as an argument. This took me a while to understand as with only one
`Verb` options class, the first run command will always execute.

    return CommandLine.Parser.Default.ParseArguments<NewCommand, UpdateCommand>(args)
      .MapResult((NewCommand opts) => RunNewAndReturnExitCode(opts),
        (UpdateCommand opts) => RunUpdateAndReturnExitCode(opts),
        errs => 1);