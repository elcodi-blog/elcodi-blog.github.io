---
layout: post
title: "The Console Component"
date: 2014-11-21 19:02:40 +0100
comments: true
categories: [One Section One Component] 
author: Berny Cantos <berny@elcodi.com>
---
The next post in the series of ["One section one component"](/blog/categories/one-section-one-component/), in support of our friend [Marc Morera](https://github.com/mmoreram) in his [#symfonywalk](https://twitter.com/hashtag/symfonywalk?f=realtime), is about the [Console component](http://symfony.com/doc/current/components/console). Marc has had some problems early in his journey, so we want to send him energy and courage for the next steps of the walk. Go Marc! You can send him words of encouragement too in his twitter account [@mmoreram](https://twitter.com/mmoreram)!

## Description

The Console component helps you create console applications. It is like `HttpRequest` + `Routing`, but for command line. With this component you can:

- Create single or multiple commands in an application.
- Define and parse command's arguments and options.
- Execute a command.
- Output information to terminal while processing a command.
- Allow automatic management of `--help` options.

## Basic classes

- **`Command`**: configuration of a command with name, parameters, options…
- **`InputArgument`** and **`InputOption`**: defines how command-line parameters and options are parsed and passed to a `Command`.
- **`InputInterface`**: allows read operations from terminal.
- **`OutputInterface`**: encapsulate terminal writing.
- **`Application`**: a group of one or more `Command`s and `Helper`s.

## Example of usage

```php
class WriteCommand extends Command
{
    protected function configure()
    {
        $this
            ->setName('write')
            ->setDescription('Writes text to a file')
            ->addArgument(
                'filename',
                InputArgument::REQUIRED,
                'Which file to write?'
            )
            ->addArgument(
                'text',
                InputArgument::OPTIONAL,
                'Which text to write?'
            )
            ->addOption(
                'yell',
                null,
                InputOption::VALUE_NONE,
                'If set, text will be upper-cased'
            )
        ;
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $filename = $input->getArgument('filename');
        $text = $input->getArgument('text');
        if ($input->getOption('yell')) {
            $text = strtoupper($text);
        }

        if (false === file_put_contents($filename, $text)) {
            throw new RuntimeException('Error writing to file.');
        }

        $output->writeln('File was successfully written.');
    }
}

$application = new Application();
$application->addCommands([
    new WriteCommand(),
]);
$application->run();
```

Now on your command line you can type `php console.php write path/to/filename "text to write"`.

Usually, the `Command` should behave like a `Controller`, this is, checking parameters and passing them to one or more services to do the real work.

You can also add a previous step of interaction with the user, so they can provide any missing or ambiguous information. This part is skipped automatically if the `--no-interaction` (or `-n`) is used.

For user interaction, you can use `OutputInterface` directly, but there are many `Helpers` you can use, being the `QuestionHelper`[^console-1] the most helpful (no pun intended).

Let's see how we can add interaction to our previous example.

```php
class WriteCommand extends Command
{
    …

    protected function interact(InputInterface $input, OutputInterface $output)
    {
        $filename = $input->getArgument('filename');
        if (null === $filename) {
            return;
        }

        if (!$this->whenBaseDirectoryExists($filename)) {
            $this->confirmDirectoryCreation($filename, $input, $output);
        }

        if ($this->whenFileAlreadyExists($filename)) {
            $this->confirmFileOverwrite($filename, $input, $output);
        }

        if (null === $input->getArgument('text')) {
            $this->askForText($input, $output);
        }
    }

    protected function whenBaseDirectoryExists($filename)
    {
        return is_dir(dirname($filename));
    }

    protected function confirmDirectoryCreation($filename, $input, $output)
    {
        $questionHelper = $this->getHelper('question');
        $question = new ConfirmationQuestion(
            'Base directory for "' . $filename . '" does not exist. Create? [y/n] ',
            true
        );

        if (!$questionHelper->ask($input, $output, $question)) {
            throw new RuntimeException('Base directory does not exist.');
        }

        mkdir(dirname($filename), 0777, true);
    }

    protected function whenFileAlreadyExists($filename)
    {
        return file_exists($filename);
    }

    protected function confirmFileOverwrite($filename, $input, $output)
    {
        $questionHelper = $this->getHelper('question');
        $question = new ConfirmationQuestion(
            'File "' . $filename . '" already exists. Overwrite? [y/n] ',
            true
        );

        if (!$questionHelper->ask($input, $output, $question)) {
            throw new RuntimeException('Can\'t write to file.');
        }
    }

    protected function askForText($input, $output)
    {
        $questionHelper = $this->getHelper('question');
        $question = new Question(
            'Please enter the content of the file: ',
            ''
        );

        $text = $questionHelper->ask($input, $output, $question);
        $input->setArgument('text', $text);
    }
}
```
[See here the full example](https://gist.github.com/xphere/546d5e90d15c1f88d750)

Now we're being requested for directory creation, file overwriting, and missing text arguments. Nice!

## Other classes

- **`Helpers`**: Extensions for `OutputInterface`, to ask questions, show progress bars or tables. You can add your own too, or check the [default ones](http://symfony.com/doc/current/components/console/helpers).

## In Symfony2 framework

Symfony2 detects `Command` classes in the `Command` directory of each bundle, and automatically adds them to the console application. As of 2.4, you can also mark services with the tag `console.command`. You can refer to this great [cookbook entry](http://symfony.com/doc/current/cookbook/console/commands_as_services.html).

## In Elcodi

In Elcodi we provide commands for things such as [querying currency exchanges](https://github.com/elcodi/Currency/blob/master/Command/CurrencyExchangeRatesPopulateCommand.php) or [populating geo entities](https://github.com/elcodi/Geo/blob/master/Command/GeoPopulateCommand.php) from third party providers.

## Conclusion

The `Console` component brings steroids to your command-line applications at nearly no cost for what you get in return. You can have multiple commands under the same application or [just one](http://symfony.com/doc/current/components/console/single_command_tool.html), you choose. If the [current PHP dependency manager](https://getcomposer.org/) uses it, why not you?

## Urls

- [Repository](https://github.com/symfony/Console)
- [Documentation](http://symfony.com/doc/current/components/console)
- [Packagist](https://packagist.org/packages/symfony/console)

[^console-1]: `QuestionHelper` is available as of Symfony 2.5, for previous versions use the, now deprecated, [`DialogHelper`](http://symfony.com/doc/2.5/components/console/helpers/dialoghelper.html).
