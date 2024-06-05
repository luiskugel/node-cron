# @idot-digital/node-cron

This is a fork of the original [node-cron](https://github.com/node-cron/node-cron) package. It adds a remove function to a task, to delete it fully.

IMPORTANT: When working with this repository, always set it up to push to github and gitlab at the same time. This is to ensure that the changes are kept in sync.

```bash
git remote set-url --add --push origin <github>
git remote set-url --add --push origin <gitlab>
```

---

The node-cron module is tiny task scheduler in pure JavaScript for node.js based on [GNU crontab](https://www.gnu.org/software/mcron/manual/html_node/Crontab-file.html). This module allows you to schedule task in node.js using full crontab syntax.

**Need a job scheduler with support for worker threads and cron syntax?** Try out the [Bree](https://github.com/breejs/bree) job scheduler!

## Getting Started

Install using npm:

```console
npm install --save @idot-digital/node-cron
```

Import node-cron and schedule a task:

- commonjs

```javascript
const cron = require('@idot-digital/node-cron');

cron.schedule('* * * * *', () => {
  console.log('running a task every minute');
});
```

- es6 (module)

```javascript
import cron from '@idot-digital/node-cron';

cron.schedule('* * * * *', () => {
  console.log('running a task every minute');
});
```

## Cron Syntax

This is a quick reference to cron syntax and also shows the options supported by node-cron.

### Allowed fields

```
 # ┌────────────── second (optional)
 # │ ┌──────────── minute
 # │ │ ┌────────── hour
 # │ │ │ ┌──────── day of month
 # │ │ │ │ ┌────── month
 # │ │ │ │ │ ┌──── day of week
 # │ │ │ │ │ │
 # │ │ │ │ │ │
 # * * * * * *
```

### Allowed values

| field        | value                             |
| ------------ | --------------------------------- |
| second       | 0-59                              |
| minute       | 0-59                              |
| hour         | 0-23                              |
| day of month | 1-31                              |
| month        | 1-12 (or names)                   |
| day of week  | 0-7 (or names, 0 or 7 are sunday) |

#### Using multiples values

You may use multiples values separated by comma:

```javascript
import cron from '@idot-digital/node-cron';

cron.schedule('1,2,4,5 * * * *', () => {
  console.log('running every minute 1, 2, 4 and 5');
});
```

#### Using ranges

You may also define a range of values:

```javascript
import cron from '@idot-digital/node-cron';

cron.schedule('1-5 * * * *', () => {
  console.log('running every minute to 1 from 5');
});
```

#### Using step values

Step values can be used in conjunction with ranges, following a range with '/' and a number. e.g: `1-10/2` that is the same as `2,4,6,8,10`. Steps are also permitted after an asterisk, so if you want to say “every two minutes”, just use `*/2`.

```javascript
import cron from '@idot-digital/node-cron';

cron.schedule('*/2 * * * *', () => {
  console.log('running a task every two minutes');
});
```

#### Using names

For month and week day you also may use names or short names. e.g:

```javascript
import cron from '@idot-digital/node-cron';

cron.schedule('* * * January,September Sunday', () => {
  console.log('running on Sundays of January and September');
});
```

Or with short names:

```javascript
import cron from '@idot-digital/node-cron';

cron.schedule('* * * Jan,Sep Sun', () => {
  console.log('running on Sundays of January and September');
});
```

## Cron methods

### Schedule

Schedules given task to be executed whenever the cron expression ticks.

Arguments:

- **expression** `string`: Cron expression
- **function** `Function`: Task to be executed
- **options** `Object`: Optional configuration for job scheduling.

#### Options

- **scheduled**: A `boolean` to set if the created task is scheduled. Default `true`;
- **recoverMissedExecutions**: A `boolean` to set if the created task should be able to recover missed executions. Default `false`;
- **timezone**: The timezone that is used for job scheduling. See [IANA time zone database](https://www.iana.org/time-zones) for valid values, such as `Asia/Shanghai`, `Asia/Kolkata`, `America/Sao_Paulo`.

**Example**:

```js
import cron from '@idot-digital/node-cron';

cron.schedule(
  '0 1 * * *',
  () => {
    console.log('Running a job at 01:00 at America/Sao_Paulo timezone');
  },
  {
    scheduled: true,
    timezone: 'America/Sao_Paulo',
  }
);
```

## ScheduledTask methods

### Start

Starts the scheduled task.

```javascript
import cron from '@idot-digital/node-cron';

const task = cron.schedule(
  '* * * * *',
  () => {
    console.log('stopped task');
  },
  {
    scheduled: false,
  }
);

task.start();
```

### Stop

The task won't be executed unless re-started.

```javascript
import cron from '@idot-digital/node-cron';

const task = cron.schedule('* * * * *', () => {
  console.log('will execute every minute until stopped');
});

task.stop();
```

### Validate

Validate that the given string is a valid cron expression.

```javascript
import cron from '@idot-digital/node-cron';

const valid = cron.validate('59 * * * *');
const invalid = cron.validate('60 * * * *');
```

### Naming tasks

You can name your tasks to make it easier to identify them in the logs.

```javascript
import cron from '@idot-digital/node-cron';

const task = cron.schedule(
  '* * * * *',
  () => {
    console.log('will execute every minute until stopped');
  },
  {
    name: 'my-task',
  }
);
```

### List tasks

You can list all the tasks that are currently running.

```javascript
import cron from '@idot-digital/node-cron';

const tasks = cron.getTasks();

for (let [key, value] of tasks.entries()) {
  console.log('key', key);
  console.log('value', value);
}
```

value is an object with the following properties:

- \_events
- \_eventsCount
- \_maxListeners
- options
- \_task
- etc...

## Issues

This is an interal fork, so please report any issues with the original package to the original repository. Otherwise the fork is not maintained.

## Contributing

Contributions to this fork are not accepted. Please contribute to the original repository.

## Contributors (of the original repository)

This project exists thanks to all the people who contribute.
<a href="https://github.com/node-cron/node-cron/graphs/contributors"><img src="https://opencollective.com/node-cron/contributors.svg?width=890&button=false" /></a>

## Backers (of the original repository)

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/node-cron#backer)]

<a href="https://opencollective.com/node-cron#backers" target="_blank"><img src="https://opencollective.com/node-cron/backers.svg?width=890"></a>

## Sponsors (of the original repository)

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/node-cron#sponsor)]

<a href="https://opencollective.com/node-cron/sponsor/0/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/1/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/2/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/3/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/4/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/5/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/6/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/7/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/8/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/node-cron/sponsor/9/website" target="_blank"><img src="https://opencollective.com/node-cron/sponsor/9/avatar.svg"></a>

## License

This fork is licensed under ISC same as the original repository. See the [LICENSE](LICENSE.md) file for details.
