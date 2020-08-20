#!/usr/bin/env node

const fs = require('fs');
const path = require('path');
const inquirer = require('inquirer');

inquirer.registerPrompt('directory', require('inquirer-select-directory'));

const run = async () => {
  const compName = process.argv[2];

  if (!compName) throw new Error('Component name should be provided');

  const response = await inquirer.prompt([
    {
      type: 'input',
      message: 'Typescript? [Y/n]',
      name: 'isTs',
      default: function() {
        return 'y';
      },
      validate: function(value) {
        const [response] = value.toLowerCase();

        if (response !== 'y' && response !== 'n') return 'Invalid answer.';
        return true;
      }
    },
    {
      type: 'input',
      message: 'Add a custom hook? [y/N]',
      name: 'hook',
      default: function() {
        return 'n';
      },
      validate: function(value) {
        const [response] = value.toLowerCase();

        if (response !== 'y' && response !== 'n') return 'Invalid answer.';

        return true;
      }
    },
    {
      type: 'directory',
      message: 'Directory',
      name: 'dir',
      basePath: '.',
    },
  ]);

  const { dir, hook, isTs } = response;
  const baseExtension = isTs === 'y' ? 'ts' : 'js';
  const addHook = hook === 'y';
  const baseDir = `${dir}/${compName}`;

  console.log(dir, hook, isTs, addHook)

  fs.mkdirSync(baseDir);
  fs.mkdirSync(`${baseDir}/components`);
  fs.writeFileSync(`${baseDir}/index.${baseExtension}`, `export { ${compName} } from './components/${compName}';\n`);
  fs.writeFileSync(`${baseDir}/components/${compName}.${baseExtension}x`, ``);
  addHook && fs.writeFileSync(`${baseDir}/use${compName}.${baseExtension}`, ``);

  console.log(`Created the component: ${compName} at ${baseDir}.`);
}

run()
  .catch((error) => {
    console.error(error);
  });
