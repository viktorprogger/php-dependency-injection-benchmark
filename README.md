# PHP Dependency Injection Benchmark

![PHP Version](https://img.shields.io/badge/PHP-8.4-blue?logo=php) ![Docker Version](https://img.shields.io/badge/Docker-%2A-lightgrey?logo=docker) ![OS](https://img.shields.io/badge/OS-ubuntu%20latest-blue?logo=ubuntu) ![Memory](https://img.shields.io/badge/Memory-500MB-blue) ![CPU](https://img.shields.io/badge/CPU-1%20Core-blue)

![PHP Dependency Injection Benchmark](images/php-dependency-injection-benchmark.jpg)

Dependency injection (DI) containers manage the creation and wiring of object dependencies, allowing applications to remain decoupled and easier to maintain.
Testing these containers verifies that they resolve dependencies correctly and perform efficiently, which is vital for application reliability.

This repository benchmarks different dependency injection containers.

**The "quickly" container is maintained by the same author as this benchmark, and the results may be unconsciously biased.**

To reduce favoritism, results are averaged over multiple runs and, where possible, multiple configurations of each container are benchmarked.

Detailed benchmark data, including environment details and dependency versions, is available in [`run_summary.yaml`](run_summary.yaml).
Raw outputs for each monthly run are archived under the [`archive`](archive) directory with date-based subdirectories.

## 📂 Test Files

The benchmark defines three dependency graphs used for testing.

- `src/classes-06.php` (`f06`): 6 classes.
- `src/classes-16.php` (`p16`): 16 classes.
- `src/classes-26.php` (`z26`): 26 classes.

The class names (`f06`, `p16`, `z26`) follow a group-unique letter plus total class count in the group to avoid overlap.

Each file contains all required classes and avoids autoloading so that container performance measurements exclude file-loading overhead.
Each test is executed with and without container startup time to measure resolution speed and initialization cost.

## 🚀 Running individual benchmarks

Build the container and execute a benchmark using docker:

```sh
docker build -t di-benchmark-php-di -f containers/php-di/Dockerfile .
docker run --rm -v "$PWD:/out" di-benchmark-php-di php benchmark.php f06 1
```

The build step prepares the image for the chosen container, and the run command executes a single run of the specified test (for example, `f06`). The resulting `results.json` file will be written to the current directory.

Some containers perform extra work during the image build; for example, `ray-di.compiled` precompiles its dependencies automatically.

## 🧩 Containers

| Name+Link | Run combinations | Description |
| --- | --- | --- |
| [Aura.Di](https://github.com/auraphp/Aura.Di) | configured transient | Configurable DI container with lazy loading and service factories |
| [Yii3 DI](https://github.com/yiisoft/di) | configured singleton, reflection singleton | PSR-11 compatible DI container with definitions and auto-wiring |
| [PHP-DI](https://github.com/PHP-DI/PHP-DI) | reflection singleton | Autowiring, annotations, and compiled container support |
| [Pimple](https://github.com/silexphp/Pimple) | configured singleton, configured transient | Lightweight closure-based container |
| [Symfony DI](https://github.com/symfony/dependency-injection) | compiled singleton | Feature-rich container with configuration and compilation |
| [Laravel Container](https://github.com/laravel/framework) | configured transient, reflection singleton, reflection transient | Framework-integrated container with automatic resolution and binding |
| [Nette DI](https://github.com/nette/di) | compiled singleton | High-performance compiled container |
| [Auryn](https://github.com/rdlowrey/auryn) | reflection transient | Auryn is a dependency injector for bootstrapping object-oriented PHP applications. |
| [Dice](https://github.com/Level-2/Dice) | configured singleton, reflection transient | A minimalist dependency injection container for PHP. |
| [Laminas ServiceManager](https://github.com/laminas/laminas-servicemanager) | reflection singleton | Factory-driven dependency injection container |
| [League Container](https://github.com/thephpleague/container) | configured transient, reflection transient | A fast and intuitive dependency injection container. |
| [Phalcon](https://github.com/phalcon/cphalcon) | configured singleton, configured transient | A PHP extension built for performance |
| [PHP (baseline)](https://www.php.net/) |  | Manual instantiation of dependencies with simple caching |
| [Quickly](https://github.com/Idrinth/quickly) | compiled singleton, configured singleton, reflection singleton | A fast dependency injection container featuring build time resolution. |
| [Ray.Di](https://github.com/ray-di/Ray.Di) | compiled transient, reflection transient | DI and AOP framework for PHP inspired by Google Guice |
| [Zen](https://github.com/woohoolabs/zen) | compiled singleton | Woohoo Labs. Zen DI Container and preload file generator |
## Latest Results

Run from 2026-11-01

### 📊 f06

Small dependency graph including 6 classes total (excluding container startup time)

![📊 f06](images/speed_comparison_without_startup_f06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 289µs, 916ns | 833µs, 34ns | 2ms, 20µs, 835ns |
| Auryn(Reflection, Transient) | ^1.4 | 368ms, 933µs, 749ns | 214ms, 124µs, 917ns | 411ms, 67µs, 962ns |
| Dice(Configured, Singleton) | ^4.0 | 541µs, 591ns | 411µs, 987ns | 698µs, 89ns |
| Dice(Reflection, Transient) | ^4.0 | 65ms, 137µs, 243ns | 58ms, 610µs, 916ns | 71ms, 95µs, 943ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 465µs, 512ns | 388µs, 145ns | 589µs, 132ns |
| Laravel(Configured, Transient) | ^12.28 | 350ms, 686µs, 168ns | 201ms, 114µs, 892ns | 413ms, 770µs, 198ns |
| Laravel(Reflection, Singleton) | ^12.28 | 3ms, 595µs, 280ns | 3ms, 428µs, 936ns | 3ms, 894µs, 90ns |
| Laravel(Reflection, Transient) | ^12.28 | 545ms, 693µs, 349ns | 495ms, 480µs, 60ns | 591ms, 475µs, 9ns |
| League(Configured, Transient) | ^5.1 | 949ms, 381µs, 589ns | 582ms, 384µs, 109ns | 1s, 162ms, 463µs, 903ns |
| League(Reflection, Transient) | ^5.1 | 651ms, 755µs, 475ns | 532ms, 325µs, 29ns | 724ms, 910µs, 974ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 469µs, 657ns | 3ms, 388µs, 881ns | 3ms, 878µs, 116ns |
| Phalcon(Configured, Singleton) | ^5 | 3ms, 186µs, 345ns | 3ms, 74µs, 169ns | 3ms, 340µs, 5ns |
| Phalcon(Configured, Transient) | ^5 | 202ms, 160µs, 406ns | 149ms, 541µs, 139ns | 257ms, 401µs, 943ns |
| Php-baseline |  | 556µs, 15ns | 432µs, 968ns | 698µs, 89ns |
| Php-di(Reflection, Singleton) | ^7.0 | 412µs, 797ns | 381µs, 946ns | 606µs, 60ns |
| Pimple(Configured, Singleton) | ^3.5 | 672µs, 650ns | 664µs, 949ns | 694µs, 36ns |
| Pimple(Configured, Transient) | ^3.5 | 76ms, 724µs, 672ns | 50ms, 34µs, 999ns | 101ms, 886µs, 34ns |
| Quickly(Compiled, Singleton) | dev-master | 830µs, 197ns | 797µs, 986ns | 873µs, 88ns |
| Quickly(Configured, Singleton) | dev-master | 1ms, 360µs, 821ns | 1ms, 347µs, 64ns | 1ms, 374µs, 959ns |
| Quickly(Reflection, Singleton) | dev-master | 670µs, 409ns | 652µs, 74ns | 757µs, 932ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 419ms, 261µs, 813ns | 1s, 185ms, 806µs, 35ns | 3s, 180ms, 516µs, 4ns |
| Ray-di(Reflection, Transient) | ^2.16 | 244ms, 963µs, 669ns | 148ms, 542µs, 165ns | 292ms, 87µs, 78ns |
| Symfony(Compiled, Singleton) | ^7.0 | 806µs, 617ns | 770µs, 92ns | 855µs, 922ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 859µs, 713ns | 813µs, 7ns | 1ms, 148µs, 939ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 662µs, 136ns | 579µs, 833ns | 1ms, 116µs, 991ns |
| Zen(Compiled, Singleton) | ^3.1 | 834µs, 512ns | 712µs, 156ns | 1ms, 574µs, 39ns |

</details>

### 🚀 f06 startup

Small dependency graph including 6 classes total (includes container startup time)

![🚀 f06 startup](images/speed_comparison_with_startup_f06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 962µs, 518ns | 1ms, 366µs, 853ns | 3ms, 772µs, 974ns |
| Auryn(Reflection, Transient) | ^1.4 | 327ms, 934µs, 885ns | 199ms, 26µs, 823ns | 419ms, 219µs, 970ns |
| Dice(Configured, Singleton) | ^4.0 | 1ms, 758µs, 503ns | 1ms, 451µs, 15ns | 2ms, 369µs, 880ns |
| Dice(Reflection, Transient) | ^4.0 | 62ms, 261µs, 319ns | 54ms, 173µs, 946ns | 70ms, 959µs, 91ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 497µs, 55ns | 397µs, 920ns | 1ms, 216µs, 173ns |
| Laravel(Configured, Transient) | ^12.28 | 365ms, 499µs, 806ns | 275ms, 340µs, 795ns | 416ms, 89µs, 57ns |
| Laravel(Reflection, Singleton) | ^12.28 | 3ms, 403µs, 496ns | 2ms, 726µs, 793ns | 5ms, 230µs, 903ns |
| Laravel(Reflection, Transient) | ^12.28 | 612ms, 479µs, 829ns | 585ms, 334µs, 62ns | 822ms, 152µs, 853ns |
| League(Configured, Transient) | ^5.1 | 1s, 55ms, 658µs, 602ns | 605ms, 319µs, 23ns | 1s, 202ms, 414µs, 989ns |
| League(Reflection, Transient) | ^5.1 | 603ms, 255µs, 558ns | 369ms, 156µs, 837ns | 720ms, 658µs, 779ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 1ms, 611µs, 280ns | 1ms, 561µs, 164ns | 1ms, 863µs, 956ns |
| Phalcon(Configured, Singleton) | ^5 | 3ms, 345µs, 417ns | 3ms, 262µs, 42ns | 3ms, 445µs, 863ns |
| Phalcon(Configured, Transient) | ^5 | 183ms, 831µs, 977ns | 125ms, 612µs, 974ns | 258ms, 535µs, 861ns |
| Php-baseline |  | 545µs, 382ns | 395µs, 59ns | 730µs, 991ns |
| Php-di(Reflection, Singleton) | ^7.0 | 1ms, 173µs, 233ns | 876µs, 903ns | 3ms, 527µs, 164ns |
| Pimple(Configured, Singleton) | ^3.5 | 734µs, 114ns | 689µs, 29ns | 883µs, 817ns |
| Pimple(Configured, Transient) | ^3.5 | 100ms, 756µs, 621ns | 99ms, 817µs, 991ns | 103ms, 846µs, 73ns |
| Quickly(Compiled, Singleton) | dev-master | 844µs, 120ns | 768µs, 184ns | 1ms, 3µs, 980ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 185µs, 630ns | 2ms, 23µs, 935ns | 3ms, 158µs, 92ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 503µs, 276ns | 1ms, 357µs, 78ns | 2ms, 461µs, 910ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 718ms, 969µs, 321ns | 1s, 94ms, 301µs, 939ns | 3s, 185ms, 298µs, 919ns |
| Ray-di(Reflection, Transient) | ^2.16 | 264ms, 828µs, 324ns | 152ms, 46µs, 918ns | 301ms, 321µs, 983ns |
| Symfony(Compiled, Singleton) | ^7.0 | 818µs, 991ns | 783µs, 920ns | 859µs, 22ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 915µs, 861ns | 727µs, 891ns | 2ms, 471µs, 923ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 72µs, 502ns | 838µs, 994ns | 3ms, 16µs, 948ns |
| Zen(Compiled, Singleton) | ^3.1 | 559µs, 43ns | 414µs, 848ns | 1ms, 685µs, 857ns |

</details>

### 📊 fin06

Small interface-based dependency graph including 6 interfaces total (excluding container startup time)

![📊 fin06](images/speed_comparison_interfaces_without_startup06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 614µs, 642ns | 1ms, 546µs, 144ns | 1ms, 764µs, 59ns |
| Dice(Configured, Singleton) | ^4.0 | 676µs, 321ns | 608µs, 205ns | 746µs, 965ns |
| Laravel(Configured, Transient) | ^12.28 | 298ms, 888µs, 993ns | 188ms, 332µs, 796ns | 392ms, 359µs, 18ns |
| League(Configured, Transient) | ^5.1 | 7s, 862ms, 366µs, 104ns | 4s, 563ms, 136µs, 100ns | 9s, 375ms, 767µs, 946ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 833µs, 818ns | 3ms, 761µs, 53ns | 4ms, 212µs, 141ns |
| Phalcon(Configured, Singleton) | ^5 | 2ms, 337µs, 265ns | 1ms, 407µs, 146ns | 3ms, 69µs, 877ns |
| Phalcon(Configured, Transient) | ^5 | 246ms, 658µs, 325ns | 232ms, 264µs, 995ns | 263ms, 447µs, 999ns |
| Pimple(Configured, Singleton) | ^3.5 | 963µs, 616ns | 957µs, 12ns | 968µs, 933ns |
| Pimple(Configured, Transient) | ^3.5 | 62ms, 37µs, 634ns | 49ms, 648µs, 46ns | 74ms, 140µs, 71ns |
| Quickly(Compiled, Singleton) | dev-master | 833µs, 392ns | 787µs, 19ns | 877µs, 857ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 984µs, 189ns | 2ms, 950µs, 906ns | 3ms, 5µs, 27ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 631ms, 153µs, 631ns | 1s, 300ms, 325µs, 870ns | 3s, 203ms, 294µs, 38ns |
| Symfony(Compiled, Singleton) | ^7.0 | 368µs, 857ns | 354µs, 51ns | 385µs, 999ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 456µs, 118ns | 421µs, 47ns | 650µs, 882ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 737µs, 762ns | 591µs, 39ns | 1ms, 195µs, 907ns |
| Zen(Compiled, Singleton) | ^3.1 | 430µs, 703ns | 373µs, 125ns | 845µs, 909ns |

</details>

### 🚀 fin06 startup

Small interface-based dependency graph including 6 interfaces total (includes container startup time)

![🚀 fin06 startup](images/speed_comparison_interfaces_with_startup06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 814µs, 556ns | 1ms, 334µs, 905ns | 3ms, 297µs, 805ns |
| Dice(Configured, Singleton) | ^4.0 | 1ms, 990µs, 818ns | 1ms, 857µs, 995ns | 2ms, 335µs, 71ns |
| Laravel(Configured, Transient) | ^12.28 | 323ms, 619µs, 318ns | 179ms, 649µs, 114ns | 418ms, 40µs, 990ns |
| League(Configured, Transient) | ^5.1 | 7s, 690ms, 349µs, 578ns | 4s, 579ms, 541µs, 921ns | 9s, 717ms, 149µs, 972ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 2ms, 79µs, 820ns | 2ms, 23µs, 935ns | 2ms, 420µs, 902ns |
| Phalcon(Configured, Singleton) | ^5 | 2ms, 416µs, 801ns | 1ms, 482µs, 963ns | 3ms, 332µs, 138ns |
| Phalcon(Configured, Transient) | ^5 | 199ms, 46µs, 373ns | 128ms, 459µs, 215ns | 248ms, 389µs, 5ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 403µs, 355ns | 1ms, 345µs, 872ns | 1ms, 614µs, 93ns |
| Pimple(Configured, Transient) | ^3.5 | 74ms, 52µs, 619ns | 73ms, 122µs, 978ns | 76ms, 220µs, 989ns |
| Quickly(Compiled, Singleton) | dev-master | 391µs, 721ns | 376µs, 939ns | 420µs, 93ns |
| Quickly(Configured, Singleton) | dev-master | 4ms, 805µs, 302ns | 4ms, 591µs, 226ns | 5ms, 712µs, 985ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 634ms, 994µs, 387ns | 1s, 548ms, 530µs, 101ns | 3s, 291ms, 859µs, 149ns |
| Symfony(Compiled, Singleton) | ^7.0 | 818µs, 443ns | 790µs, 834ns | 852µs, 823ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 631µs, 618ns | 475µs, 883ns | 1ms, 947µs, 879ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 107µs, 120ns | 850µs, 915ns | 3ms, 201µs, 961ns |
| Zen(Compiled, Singleton) | ^3.1 | 1ms, 76µs, 197ns | 833µs, 34ns | 3ms, 31µs, 969ns |

</details>

### 📊 p16

Medium size dependency graph including 16 classes total. Skipped for the slowest DI-Containers for runtime reasons. (excluding container startup time)

![📊 p16](images/speed_comparison_without_startup_p16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 4ms, 595µs, 88ns | 2ms, 749µs, 204ns | 5ms, 532µs, 26ns |
| Dice(Configured, Singleton) | ^4.0 | 762µs, 534ns | 442µs, 28ns | 919µs, 103ns |
| Dice(Reflection, Transient) | ^4.0 | 8s, 456ms, 590µs, 127ns | 4s, 908ms, 733µs, 129ns | 10s, 82ms, 941µs, 55ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 393µs, 56ns | 374µs, 794ns | 473µs, 22ns |
| Laravel(Reflection, Singleton) | ^12.28 | 2ms, 841µs, 186ns | 1ms, 888µs, 990ns | 3ms, 962µs, 993ns |
| Laravel(Reflection, Transient) | ^12.28 | 75s, 886ms, 352µs, 968ns | 39s, 591ms, 372µs, 966ns | 84s, 477ms, 833µs, 32ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 2ms, 675µs, 32ns | 2ms, 449µs, 989ns | 3ms, 184µs, 80ns |
| Phalcon(Configured, Singleton) | ^5 | 2ms, 467µs, 60ns | 1ms, 308µs, 917ns | 3ms, 207µs, 921ns |
| Php-baseline |  | 594µs, 353ns | 293µs, 16ns | 776µs, 52ns |
| Php-di(Reflection, Singleton) | ^7.0 | 482µs, 678ns | 396µs, 966ns | 658µs, 988ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 302µs, 146ns | 1ms, 263µs, 141ns | 1ms, 350µs, 164ns |
| Pimple(Configured, Transient) | ^3.5 | 12s, 81ms, 754µs, 493ns | 7s, 158ms, 303µs, 976ns | 14s, 288ms, 32µs, 54ns |
| Quickly(Compiled, Singleton) | dev-master | 827µs, 2ns | 796µs, 79ns | 941µs, 38ns |
| Quickly(Configured, Singleton) | dev-master | 706µs, 982ns | 687µs, 122ns | 742µs, 912ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 491µs, 689ns | 1ms, 352µs, 787ns | 2ms, 148µs, 151ns |
| Symfony(Compiled, Singleton) | ^7.0 | 902µs, 366ns | 786µs, 66ns | 1ms, 257µs, 896ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 744µs, 676ns | 688µs, 76ns | 1ms, 67µs, 161ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 879µs, 287ns | 787µs, 973ns | 1ms, 549µs, 5ns |
| Zen(Compiled, Singleton) | ^3.1 | 829µs, 982ns | 729µs, 84ns | 1ms, 530µs, 885ns |

</details>

### 🚀 p16 startup

Medium size dependency graph including 16 classes total. Skipped for the slowest DI-Containers for runtime reasons. (includes container startup time)

![🚀 p16 startup](images/speed_comparison_with_startup_p16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 6ms, 181µs, 812ns | 3ms, 756µs, 999ns | 7ms, 366µs, 180ns |
| Dice(Configured, Singleton) | ^4.0 | 2ms, 39µs, 957ns | 1ms, 269µs, 817ns | 2ms, 930µs, 879ns |
| Dice(Reflection, Transient) | ^4.0 | 8s, 939ms, 585µs, 590ns | 5s, 223ms, 709µs, 821ns | 10s, 168ms, 376µs, 922ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 1ms, 6µs, 865ns | 859µs, 22ns | 2ms, 123µs, 832ns |
| Laravel(Reflection, Singleton) | ^12.28 | 4ms, 524µs, 397ns | 2ms, 774µs, 953ns | 7ms, 787µs, 942ns |
| Laravel(Reflection, Transient) | ^12.28 | 62s, 303ms, 796µs, 124ns | 38s, 843ms, 889µs, 951ns | 83s, 445ms, 163µs, 11ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 533µs, 577ns | 3ms, 412µs, 961ns | 4ms, 411µs, 935ns |
| Phalcon(Configured, Singleton) | ^5 | 2ms, 913µs, 403ns | 1ms, 496µs, 76ns | 3ms, 957µs, 33ns |
| Php-baseline |  | 544µs, 500ns | 292µs, 62ns | 761µs, 985ns |
| Php-di(Reflection, Singleton) | ^7.0 | 1ms, 193µs, 380ns | 917µs, 911ns | 3ms, 527µs, 879ns |
| Pimple(Configured, Singleton) | ^3.5 | 718µs, 832ns | 684µs, 22ns | 916µs, 4ns |
| Pimple(Configured, Transient) | ^3.5 | 12s, 809ms, 895µs, 467ns | 7s, 23ms, 792µs, 982ns | 14s, 379ms, 172µs, 801ns |
| Quickly(Compiled, Singleton) | dev-master | 552µs, 272ns | 515µs, 937ns | 622µs, 987ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 216µs, 863ns | 2ms, 56µs, 837ns | 3ms, 165µs, 960ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 557µs, 612ns | 1ms, 428µs, 127ns | 2ms, 441µs, 883ns |
| Symfony(Compiled, Singleton) | ^7.0 | 804µs, 209ns | 783µs, 920ns | 854µs, 969ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 927µs, 424ns | 716µs, 924ns | 2ms, 568µs, 6ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 127µs, 52ns | 890µs, 16ns | 3ms, 148µs, 78ns |
| Zen(Compiled, Singleton) | ^3.1 | 585µs, 341ns | 418µs, 186ns | 1ms, 864µs, 194ns |

</details>

### 📊 pin16

Medium size interface-based dependency graph including 16 interfaces total. Skipped for the slowest DI-Containers for runtime reasons. (excluding container startup time)

![📊 pin16](images/speed_comparison_interfaces_without_startup16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 541µs, 304ns | 957µs, 12ns | 1ms, 907µs, 110ns |
| Dice(Configured, Singleton) | ^4.0 | 763µs, 916ns | 441µs, 74ns | 967µs, 979ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 882µs, 598ns | 3ms, 770µs, 112ns | 4ms, 392µs, 862ns |
| Phalcon(Configured, Singleton) | ^5 | 2ms, 593µs, 588ns | 1ms, 521µs, 825ns | 3ms, 87µs, 997ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 328µs, 396ns | 1ms, 245µs, 975ns | 1ms, 894µs, 950ns |
| Pimple(Configured, Transient) | ^3.5 | 11s, 867ms, 498µs, 183ns | 6s, 932ms, 370µs, 901ns | 14s, 300ms, 752µs, 162ns |
| Quickly(Compiled, Singleton) | dev-master | 407µs, 814ns | 390µs, 52ns | 474µs, 929ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 45µs, 798ns | 2ms, 17µs, 21ns | 2ms, 136µs, 945ns |
| Symfony(Compiled, Singleton) | ^7.0 | 792µs, 121ns | 772µs, 953ns | 826µs, 835ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 662µs, 302ns | 617µs, 27ns | 923µs, 871ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 8µs, 820ns | 918µs, 149ns | 1ms, 785µs, 993ns |
| Zen(Compiled, Singleton) | ^3.1 | 848µs, 531ns | 742µs, 197ns | 1ms, 621µs, 961ns |

</details>

### 🚀 pin16 startup

Medium size interface-based dependency graph including 16 interfaces total. Skipped for the slowest DI-Containers for runtime reasons. (includes container startup time)

![🚀 pin16 startup](images/speed_comparison_interfaces_with_startup16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 3ms, 53µs, 92ns | 1ms, 914µs, 24ns | 3ms, 462µs, 791ns |
| Dice(Configured, Singleton) | ^4.0 | 2ms, 336µs, 215ns | 1ms, 293µs, 897ns | 3ms, 392µs, 934ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 1ms, 894µs, 450ns | 1ms, 845µs, 836ns | 2ms, 201µs, 795ns |
| Phalcon(Configured, Singleton) | ^5 | 2ms, 939µs, 105ns | 1ms, 605µs, 33ns | 3ms, 519µs, 58ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 410µs, 79ns | 1ms, 343µs, 11ns | 1ms, 638µs, 889ns |
| Pimple(Configured, Transient) | ^3.5 | 11s, 529ms, 952µs, 692ns | 7s, 440ms, 613µs, 985ns | 14s, 336ms, 382µs, 865ns |
| Quickly(Compiled, Singleton) | dev-master | 832µs, 796ns | 798µs, 940ns | 899µs, 76ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 333µs, 974ns | 2ms, 227µs, 67ns | 2ms, 948µs, 45ns |
| Symfony(Compiled, Singleton) | ^7.0 | 601µs, 100ns | 581µs, 26ns | 631µs, 93ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 623µs, 106ns | 419µs, 139ns | 1ms, 708µs, 984ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 143µs, 407ns | 898µs, 838ns | 3ms, 254µs, 175ns |
| Zen(Compiled, Singleton) | ^3.1 | 577µs, 926ns | 411µs, 33ns | 1ms, 729µs, 11ns |

</details>

### 📊 z26

Large dependency graph including a total of 26 classes. Skipped for all but the fastest DI-Containers for runtime reasons. (excluding container startup time)

![📊 z26](images/speed_comparison_without_startup_z26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 841µs, 498ns | 784µs, 873ns | 1ms, 35µs, 213ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 2ms, 560µs, 997ns | 2ms, 429µs, 962ns | 3ms, 152µs, 132ns |
| Php-di(Reflection, Singleton) | ^7.0 | 426µs, 101ns | 384µs, 807ns | 705µs, 957ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 260µs, 685ns | 1ms, 238µs, 822ns | 1ms, 288µs, 890ns |
| Quickly(Compiled, Singleton) | dev-master | 845µs, 718ns | 813µs, 7ns | 942µs, 945ns |
| Quickly(Configured, Singleton) | dev-master | 1ms, 361µs, 322ns | 1ms, 314µs, 163ns | 1ms, 472µs, 949ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 373µs, 863ns | 1ms, 324µs, 892ns | 1ms, 675µs, 128ns |
| Symfony(Compiled, Singleton) | ^7.0 | 889µs, 658ns | 773µs, 191ns | 1ms, 308µs, 917ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 483µs, 12ns | 431µs, 60ns | 745µs, 58ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 975µs, 990ns | 865µs, 936ns | 1ms, 754µs, 45ns |
| Zen(Compiled, Singleton) | ^3.1 | 684µs, 309ns | 565µs, 52ns | 1ms, 215µs, 934ns |

</details>

### 🚀 z26 startup

Large dependency graph including a total of 26 classes. Skipped for all but the fastest DI-Containers for runtime reasons. (includes container startup time)

![🚀 z26 startup](images/speed_comparison_with_startup_z26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 935µs, 387ns | 803µs, 947ns | 1ms, 825µs, 94ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 2ms, 875µs, 471ns | 2ms, 810µs, 955ns | 3ms, 249µs, 883ns |
| Php-di(Reflection, Singleton) | ^7.0 | 1ms, 288µs, 32ns | 1ms, 165ns | 3ms, 556µs, 966ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 451µs, 873ns | 1ms, 351µs, 118ns | 1ms, 708µs, 984ns |
| Quickly(Compiled, Singleton) | dev-master | 369µs, 930ns | 353µs, 97ns | 442µs, 28ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 338µs, 814ns | 2ms, 91µs, 884ns | 3ms, 232µs, 955ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 708µs, 674ns | 1ms, 468µs, 896ns | 2ms, 530µs, 813ns |
| Symfony(Compiled, Singleton) | ^7.0 | 414µs, 609ns | 398µs, 874ns | 475µs, 883ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 677µs, 156ns | 514µs, 984ns | 2ms, 13µs, 921ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 897µs, 812ns | 705µs, 957ns | 2ms, 444µs, 982ns |
| Zen(Compiled, Singleton) | ^3.1 | 1ms, 151µs, 704ns | 899µs, 76ns | 3ms, 150µs, 939ns |

</details>

### 📊 zin26

Large interface-based dependency graph including a total of 26 interfaces. Skipped for all but the fastest DI-Containers for runtime reasons. (excluding container startup time)

![📊 zin26](images/speed_comparison_interfaces_without_startup26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 222µs, 584ns | 3ms, 139µs, 19ns | 3ms, 597µs, 974ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 290µs, 345ns | 1ms, 266µs, 2ns | 1ms, 389µs, 26ns |
| Quickly(Compiled, Singleton) | dev-master | 823µs, 497ns | 789µs, 880ns | 910µs, 997ns |
| Quickly(Configured, Singleton) | dev-master | 3ms, 788µs, 590ns | 3ms, 755µs, 92ns | 3ms, 827µs, 95ns |
| Symfony(Compiled, Singleton) | ^7.0 | 552µs, 296ns | 514µs, 984ns | 611µs, 66ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 874µs, 614ns | 786µs, 66ns | 1ms, 346µs, 111ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 926µs, 232ns | 794µs, 887ns | 1ms, 857µs, 42ns |
| Zen(Compiled, Singleton) | ^3.1 | 648µs, 736ns | 568µs, 866ns | 1ms, 290µs, 82ns |

</details>

### 🚀 zin26 startup

Large interface-based dependency graph including a total of 26 interfaces. Skipped for all but the fastest DI-Containers for runtime reasons. (includes container startup time)

![🚀 zin26 startup](images/speed_comparison_interfaces_with_startup26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Nette-di(Compiled, Singleton) | ^3.2 | 2ms, 153µs, 420ns | 1ms, 921µs, 892ns | 3ms, 300µs, 905ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 88µs, 118ns | 1ms, 48µs, 88ns | 1ms, 286µs, 29ns |
| Quickly(Compiled, Singleton) | dev-master | 907µs, 111ns | 867µs, 128ns | 1ms, 38µs, 74ns |
| Quickly(Configured, Singleton) | dev-master | 4ms, 697µs, 251ns | 4ms, 545µs, 927ns | 5ms, 621µs, 194ns |
| Symfony(Compiled, Singleton) | ^7.0 | 905µs, 609ns | 777µs, 6ns | 1ms, 268µs, 148ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 1ms, 250µs, 410ns | 998µs, 20ns | 3ms, 317µs, 117ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 247µs, 811ns | 972µs, 32ns | 3ms, 475µs, 904ns |
| Zen(Compiled, Singleton) | ^3.1 | 1ms, 178µs, 264ns | 935µs, 77ns | 3ms, 158µs, 807ns |

</details>

Questions, issues, and new containers are welcome!
