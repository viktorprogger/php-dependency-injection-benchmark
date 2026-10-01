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

Run from 2026-10-01

### 📊 f06

Small dependency graph including 6 classes total (excluding container startup time)

![📊 f06](images/speed_comparison_without_startup_f06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 2ms, 254µs, 652ns | 1ms, 595µs, 973ns | 3ms, 129µs, 5ns |
| Auryn(Reflection, Transient) | ^1.4 | 361ms, 956µs | 202ms, 514µs, 886ns | 413ms, 690µs, 90ns |
| Dice(Configured, Singleton) | ^4.0 | 742µs, 888ns | 622µs, 987ns | 860µs, 929ns |
| Dice(Reflection, Transient) | ^4.0 | 65ms, 4µs, 706ns | 54ms, 473µs, 876ns | 73ms, 750µs, 972ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 557µs, 184ns | 538µs, 110ns | 576µs, 19ns |
| Laravel(Configured, Transient) | ^12.28 | 367ms, 265µs, 510ns | 210ms, 565µs, 90ns | 419ms, 14µs, 930ns |
| Laravel(Reflection, Singleton) | ^12.28 | 3ms, 626µs, 751ns | 3ms, 453µs, 16ns | 3ms, 967µs, 46ns |
| Laravel(Reflection, Transient) | ^12.28 | 341ms, 272µs, 664ns | 278ms, 522µs, 968ns | 402ms, 231µs, 931ns |
| League(Configured, Transient) | ^5.1 | 1s, 66ms, 970µs, 610ns | 869ms, 708µs, 61ns | 1s, 213ms, 361µs, 978ns |
| League(Reflection, Transient) | ^5.1 | 623ms, 120µs, 498ns | 401ms, 453µs, 971ns | 759ms, 856µs, 939ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 394µs, 508ns | 3ms, 314µs, 971ns | 3ms, 836µs, 154ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 948µs, 759ns | 4ms, 610µs, 61ns | 5ms, 311µs, 965ns |
| Phalcon(Configured, Transient) | ^5 | 265ms, 223µs, 312ns | 159ms, 227µs, 132ns | 292ms, 450µs, 189ns |
| Php-baseline |  | 725µs, 293ns | 576µs, 19ns | 876µs, 903ns |
| Php-di(Reflection, Singleton) | ^7.0 | 865µs, 268ns | 810µs, 146ns | 1ms, 217µs, 842ns |
| Pimple(Configured, Singleton) | ^3.5 | 947µs, 332ns | 900µs, 983ns | 984µs, 907ns |
| Pimple(Configured, Transient) | ^3.5 | 92ms, 326µs, 593ns | 82ms, 333µs, 87ns | 103ms, 811µs, 979ns |
| Quickly(Compiled, Singleton) | dev-master | 466µs, 847ns | 442µs, 28ns | 488µs, 996ns |
| Quickly(Configured, Singleton) | dev-master | 920µs, 367ns | 908µs, 851ns | 946µs, 44ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 47µs, 563ns | 1ms, 35µs, 928ns | 1ms, 111µs, 984ns |
| Ray-di(Compiled, Transient) | ^2.16 | 3s, 89ms, 510µs, 846ns | 1s, 196ms, 203µs, 947ns | 3s, 596ms, 313µs, 953ns |
| Ray-di(Reflection, Transient) | ^2.16 | 265ms, 511µs, 202ns | 162ms, 713µs, 50ns | 332ms, 5µs, 977ns |
| Symfony(Compiled, Singleton) | ^7.0 | 788µs, 116ns | 760µs, 793ns | 818µs, 967ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 834µs, 608ns | 787µs, 19ns | 1ms, 122µs, 951ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 680µs, 160ns | 627µs, 40ns | 1ms, 55µs, 2ns |
| Zen(Compiled, Singleton) | ^3.1 | 444µs, 173ns | 392µs, 913ns | 833µs, 988ns |

</details>

### 🚀 f06 startup

Small dependency graph including 6 classes total (includes container startup time)

![🚀 f06 startup](images/speed_comparison_with_startup_f06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 929µs, 688ns | 1ms, 350µs, 164ns | 3ms, 648µs, 996ns |
| Auryn(Reflection, Transient) | ^1.4 | 351ms, 161µs, 694ns | 303ms, 920µs, 984ns | 411ms, 274µs, 909ns |
| Dice(Configured, Singleton) | ^4.0 | 1ms, 906µs, 418ns | 1ms, 755µs, 952ns | 2ms, 222µs, 61ns |
| Dice(Reflection, Transient) | ^4.0 | 74ms, 765µs, 992ns | 72ms, 914µs, 123ns | 80ms, 821µs, 37ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 953µs, 412ns | 802µs, 993ns | 1ms, 993µs, 179ns |
| Laravel(Configured, Transient) | ^12.28 | 334ms, 935µs, 69ns | 210ms, 311µs, 889ns | 414ms, 699µs, 77ns |
| Laravel(Reflection, Singleton) | ^12.28 | 3ms, 920µs, 912ns | 3ms, 507µs, 852ns | 5ms, 155µs, 86ns |
| Laravel(Reflection, Transient) | ^12.28 | 583ms, 43µs, 932ns | 578ms, 296µs, 184ns | 587ms, 738µs, 37ns |
| League(Configured, Transient) | ^5.1 | 976ms, 240µs, 777ns | 641ms, 52µs, 7ns | 1s, 155ms, 470µs, 848ns |
| League(Reflection, Transient) | ^5.1 | 629ms, 779µs, 624ns | 378ms, 39µs, 121ns | 722ms, 82µs, 138ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 126µs, 883ns | 2ms, 882µs, 957ns | 3ms, 757µs, 953ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 536µs, 390ns | 3ms, 654µs, 3ns | 5ms, 459µs, 70ns |
| Phalcon(Configured, Transient) | ^5 | 254ms, 603µs, 505ns | 149ms, 429µs, 82ns | 289ms, 888µs, 143ns |
| Php-baseline |  | 561µs, 690ns | 540µs, 18ns | 580µs, 72ns |
| Php-di(Reflection, Singleton) | ^7.0 | 1ms, 175µs, 570ns | 922µs, 918ns | 3ms, 339µs, 52ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 386µs, 117ns | 1ms, 346µs, 111ns | 1ms, 568µs, 78ns |
| Pimple(Configured, Transient) | ^3.5 | 76ms, 872µs, 420ns | 50ms, 668µs, 954ns | 102ms, 653µs, 980ns |
| Quickly(Compiled, Singleton) | dev-master | 824µs, 999ns | 802µs, 993ns | 850µs, 915ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 212µs, 429ns | 2ms, 88µs, 69ns | 2ms, 958µs, 59ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 657µs, 9ns | 1ms, 503µs, 944ns | 2ms, 454µs, 42ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 790ms, 643µs, 692ns | 1s, 645ms, 75µs, 82ns | 3s, 501ms, 574µs, 993ns |
| Ray-di(Reflection, Transient) | ^2.16 | 280ms, 785µs, 202ns | 167ms, 527µs, 914ns | 302ms, 933µs, 931ns |
| Symfony(Compiled, Singleton) | ^7.0 | 595µs, 21ns | 575µs, 65ns | 618µs, 219ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 598µs, 144ns | 442µs, 28ns | 1ms, 707µs, 77ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 569µs, 81ns | 431µs, 60ns | 1ms, 733µs, 64ns |
| Zen(Compiled, Singleton) | ^3.1 | 488µs, 686ns | 359µs, 58ns | 1ms, 556µs, 873ns |

</details>

### 📊 fin06

Small interface-based dependency graph including 6 interfaces total (excluding container startup time)

![📊 fin06](images/speed_comparison_interfaces_without_startup06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 657µs, 772ns | 1ms, 581µs, 907ns | 1ms, 884µs, 937ns |
| Dice(Configured, Singleton) | ^4.0 | 756µs, 382ns | 638µs, 961ns | 894µs, 69ns |
| Laravel(Configured, Transient) | ^12.28 | 357ms, 626µs, 247ns | 295ms, 887µs, 947ns | 381ms, 954µs, 908ns |
| League(Configured, Transient) | ^5.1 | 8s, 362ms, 603µs, 378ns | 5s, 491ms, 694µs, 927ns | 9s, 648ms, 816µs, 108ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 797µs, 30ns | 3ms, 719µs, 91ns | 4ms, 214µs, 48ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 854µs, 416ns | 4ms, 537µs, 105ns | 5ms, 201µs, 101ns |
| Phalcon(Configured, Transient) | ^5 | 234ms, 489µs, 369ns | 145ms, 680µs, 904ns | 291ms, 233µs, 62ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 355µs, 719ns | 1ms, 249µs, 74ns | 1ms, 792µs, 907ns |
| Pimple(Configured, Transient) | ^3.5 | 79ms, 885µs, 792ns | 74ms, 901µs, 103ns | 84ms, 514µs, 141ns |
| Quickly(Compiled, Singleton) | dev-master | 461µs, 411ns | 442µs, 981ns | 475µs, 883ns |
| Quickly(Configured, Singleton) | dev-master | 3ms, 864µs, 502ns | 3ms, 798µs, 7ns | 4ms, 28µs, 797ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 372ms, 652µs, 602ns | 1s, 166ms, 32µs, 75ns | 3s, 464ms, 646µs, 100ns |
| Symfony(Compiled, Singleton) | ^7.0 | 813µs, 269ns | 781µs, 59ns | 848µs, 54ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 832µs, 605ns | 790µs, 834ns | 1ms, 130µs, 104ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 889µs, 515ns | 812µs, 53ns | 1ms, 492µs, 23ns |
| Zen(Compiled, Singleton) | ^3.1 | 641µs, 608ns | 576µs, 19ns | 1ms, 111µs, 30ns |

</details>

### 🚀 fin06 startup

Small interface-based dependency graph including 6 interfaces total (includes container startup time)

![🚀 fin06 startup](images/speed_comparison_interfaces_with_startup06.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 697µs, 468ns | 996µs, 828ns | 3ms, 398µs, 180ns |
| Dice(Configured, Singleton) | ^4.0 | 1ms, 752µs, 495ns | 1ms, 390µs, 933ns | 2ms, 255µs, 916ns |
| Laravel(Configured, Transient) | ^12.28 | 368ms, 570µs, 637ns | 294ms, 257µs, 164ns | 391ms, 437µs, 53ns |
| League(Configured, Transient) | ^5.1 | 8s, 601ms, 884µs, 913ns | 6s, 406ms, 152µs, 963ns | 9s, 640ms, 912µs, 55ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 4ms, 48µs, 180ns | 3ms, 885µs, 984ns | 4ms, 940µs, 32ns |
| Phalcon(Configured, Singleton) | ^5 | 3ms, 26µs, 819ns | 2ms, 232µs, 789ns | 3ms, 817µs, 81ns |
| Phalcon(Configured, Transient) | ^5 | 256ms, 85µs, 634ns | 144ms, 956µs, 827ns | 300ms, 626µs, 39ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 430µs, 416ns | 1ms, 391µs, 887ns | 1ms, 615µs, 47ns |
| Pimple(Configured, Transient) | ^3.5 | 103ms, 116µs, 750ns | 101ms, 728µs, 916ns | 104ms, 485µs, 988ns |
| Quickly(Compiled, Singleton) | dev-master | 801µs, 86ns | 775µs, 814ns | 844µs, 1ns |
| Quickly(Configured, Singleton) | dev-master | 5ms, 306µs, 196ns | 4ms, 424µs, 810ns | 9ms, 787µs, 82ns |
| Ray-di(Compiled, Transient) | ^2.16 | 2s, 898ms, 454µs, 332ns | 2s, 191ms, 990µs, 852ns | 3s, 313ms, 798µs, 904ns |
| Symfony(Compiled, Singleton) | ^7.0 | 794µs, 458ns | 775µs, 98ns | 806µs, 808ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 1ms, 137µs, 757ns | 916µs, 4ns | 3ms, 15µs, 995ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 860µs, 691ns | 665µs, 903ns | 2ms, 411µs, 127ns |
| Zen(Compiled, Singleton) | ^3.1 | 946µs, 450ns | 727µs, 176ns | 2ms, 737µs, 45ns |

</details>

### 📊 p16

Medium size dependency graph including 16 classes total. Skipped for the slowest DI-Containers for runtime reasons. (excluding container startup time)

![📊 p16](images/speed_comparison_without_startup_p16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 4ms, 925µs, 894ns | 2ms, 676µs, 963ns | 7ms, 45µs, 30ns |
| Dice(Configured, Singleton) | ^4.0 | 780µs, 987ns | 542µs, 879ns | 1ms, 86µs, 950ns |
| Dice(Reflection, Transient) | ^4.0 | 9s, 645ms, 156µs, 502ns | 6s, 35ms, 191µs, 59ns | 10s, 735ms, 987µs, 901ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 418µs, 782ns | 401µs, 973ns | 463µs, 8ns |
| Laravel(Reflection, Singleton) | ^12.28 | 3ms, 389µs, 954ns | 2ms, 281µs, 188ns | 3ms, 996µs, 849ns |
| Laravel(Reflection, Transient) | ^12.28 | 66s, 402ms, 909µs, 898ns | 42s, 800ms, 307µs, 35ns | 82s, 141ms, 808µs, 986ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 408µs, 145ns | 3ms, 325µs, 223ns | 3ms, 907µs, 918ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 790µs, 878ns | 2ms, 312µs, 898ns | 5ms, 311µs, 965ns |
| Php-baseline |  | 622µs, 463ns | 410µs, 79ns | 854µs, 15ns |
| Php-di(Reflection, Singleton) | ^7.0 | 416µs, 16ns | 386µs, 953ns | 628µs, 948ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 319µs, 122ns | 1ms, 303µs, 195ns | 1ms, 368µs, 999ns |
| Pimple(Configured, Transient) | ^3.5 | 13s, 45ms, 835µs, 399ns | 9s, 851ms, 610µs, 898ns | 14s, 283ms, 372µs, 879ns |
| Quickly(Compiled, Singleton) | dev-master | 839µs, 543ns | 818µs, 967ns | 873µs, 88ns |
| Quickly(Configured, Singleton) | dev-master | 1ms, 345µs, 229ns | 1ms, 312µs, 971ns | 1ms, 372µs, 98ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 586µs, 866ns | 1ms, 312µs, 971ns | 2ms, 501µs, 10ns |
| Symfony(Compiled, Singleton) | ^7.0 | 1ms, 69µs, 116ns | 1ms, 52µs, 141ns | 1ms, 92µs, 910ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 654µs, 554ns | 614µs, 166ns | 890µs, 16ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 868µs, 105ns | 783µs, 920ns | 1ms, 456µs, 22ns |
| Zen(Compiled, Singleton) | ^3.1 | 643µs, 205ns | 571µs, 966ns | 1ms, 116µs, 37ns |

</details>

### 🚀 p16 startup

Medium size dependency graph including 16 classes total. Skipped for the slowest DI-Containers for runtime reasons. (includes container startup time)

![🚀 p16 startup](images/speed_comparison_with_startup_p16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 6ms, 606µs, 197ns | 5ms, 250µs, 930ns | 7ms, 233µs, 858ns |
| Dice(Configured, Singleton) | ^4.0 | 2ms, 186µs, 632ns | 1ms, 708µs, 984ns | 2ms, 960µs, 920ns |
| Dice(Reflection, Transient) | ^4.0 | 9s, 645ms, 298µs, 385ns | 6s, 32ms, 974µs, 4ns | 10s, 428ms, 174µs, 18ns |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 1ms, 210µs, 713ns | 964µs, 879ns | 3ms, 138µs, 65ns |
| Laravel(Reflection, Singleton) | ^12.28 | 4ms, 739µs, 999ns | 2ms, 833µs, 127ns | 5ms, 439µs, 996ns |
| Laravel(Reflection, Transient) | ^12.28 | 72s, 966ms, 871µs, 285ns | 40s, 327ms, 26µs, 128ns | 83s, 346ms, 762µs, 895ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 578µs, 519ns | 3ms, 458µs, 23ns | 4ms, 902ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 573µs, 917ns | 2ms, 485µs, 990ns | 5ms, 703µs, 926ns |
| Php-baseline |  | 529µs, 146ns | 308µs, 990ns | 675µs, 916ns |
| Php-di(Reflection, Singleton) | ^7.0 | 638µs, 580ns | 464µs, 916ns | 2ms, 24µs, 888ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 472µs, 735ns | 1ms, 409µs, 53ns | 1ms, 704µs, 931ns |
| Pimple(Configured, Transient) | ^3.5 | 12s, 762ms, 664µs, 890ns | 10s, 593ms, 586µs, 921ns | 14s, 156ms, 688µs, 928ns |
| Quickly(Compiled, Singleton) | dev-master | 836µs, 14ns | 811µs, 100ns | 936µs, 985ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 153µs, 86ns | 2ms, 27µs, 34ns | 2ms, 861µs, 976ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 526µs, 975ns | 1ms, 417µs, 875ns | 2ms, 317µs, 905ns |
| Symfony(Compiled, Singleton) | ^7.0 | 806µs, 474ns | 768µs, 899ns | 926µs, 971ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 1ms, 210µs, 927ns | 960µs, 111ns | 3ms, 35µs, 68ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 143µs, 74ns | 934µs, 839ns | 2ms, 913µs, 951ns |
| Zen(Compiled, Singleton) | ^3.1 | 1ms, 96µs, 391ns | 855µs, 922ns | 2ms, 974µs, 987ns |

</details>

### 📊 pin16

Medium size interface-based dependency graph including 16 interfaces total. Skipped for the slowest DI-Containers for runtime reasons. (excluding container startup time)

![📊 pin16](images/speed_comparison_interfaces_without_startup16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 1ms, 656µs, 7ns | 1ms, 91µs, 3ns | 2ms, 226µs, 114ns |
| Dice(Configured, Singleton) | ^4.0 | 750µs, 88ns | 424µs, 146ns | 938µs, 892ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 2ms, 780µs, 508ns | 2ms, 726µs, 78ns | 3ms, 51µs, 996ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 479µs, 455ns | 2ms, 193µs, 212ns | 5ms, 479µs, 97ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 233µs, 601ns | 1ms, 206µs, 874ns | 1ms, 251µs, 935ns |
| Pimple(Configured, Transient) | ^3.5 | 12s, 407ms, 961µs, 988ns | 7s, 99ms, 38µs, 839ns | 14s, 251ms, 59µs, 55ns |
| Quickly(Compiled, Singleton) | dev-master | 595µs, 92ns | 578µs, 165ns | 627µs, 994ns |
| Quickly(Configured, Singleton) | dev-master | 1ms, 882µs, 553ns | 1ms, 868µs, 9ns | 1ms, 926µs, 898ns |
| Symfony(Compiled, Singleton) | ^7.0 | 592µs, 803ns | 576µs, 972ns | 631µs, 809ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 858µs, 402ns | 811µs, 100ns | 1ms, 188µs, 993ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 881µs, 958ns | 793µs, 933ns | 1ms, 543µs, 998ns |
| Zen(Compiled, Singleton) | ^3.1 | 866µs, 794ns | 768µs, 899ns | 1ms, 530µs, 885ns |

</details>

### 🚀 pin16 startup

Medium size interface-based dependency graph including 16 interfaces total. Skipped for the slowest DI-Containers for runtime reasons. (includes container startup time)

![🚀 pin16 startup](images/speed_comparison_interfaces_with_startup16.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Aura-di(Configured, Transient) | ^5.0 | 3ms, 181µs, 147ns | 2ms, 501µs, 10ns | 3ms, 487µs, 825ns |
| Dice(Configured, Singleton) | ^4.0 | 2ms, 117µs, 800ns | 1ms, 353µs, 25ns | 3ms, 856µs, 897ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 4ms, 13µs, 800ns | 3ms, 883µs, 123ns | 4ms, 376µs, 888ns |
| Phalcon(Configured, Singleton) | ^5 | 4ms, 525µs, 637ns | 2ms, 304µs, 77ns | 5ms, 608µs, 81ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 461µs, 5ns | 1ms, 358µs, 32ns | 1ms, 735µs, 925ns |
| Pimple(Configured, Transient) | ^3.5 | 13s, 428ms, 807µs, 449ns | 11s, 308ms, 781µs, 862ns | 14s, 262ms, 93µs, 782ns |
| Quickly(Compiled, Singleton) | dev-master | 827µs, 598ns | 810µs, 146ns | 866µs, 889ns |
| Quickly(Configured, Singleton) | dev-master | 5ms, 27µs, 437ns | 3ms, 569µs, 841ns | 6ms, 64µs, 891ns |
| Symfony(Compiled, Singleton) | ^7.0 | 805µs, 211ns | 769µs, 853ns | 827µs, 74ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 631µs, 737ns | 481µs, 128ns | 1ms, 729µs, 965ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 148µs, 104ns | 910µs, 43ns | 3ms, 3µs, 120ns |
| Zen(Compiled, Singleton) | ^3.1 | 506µs, 424ns | 378µs, 131ns | 1ms, 546µs, 144ns |

</details>

### 📊 z26

Large dependency graph including a total of 26 classes. Skipped for all but the fastest DI-Containers for runtime reasons. (excluding container startup time)

![📊 z26](images/speed_comparison_without_startup_z26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 420µs, 165ns | 401µs, 20ns | 513µs, 792ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 615µs, 427ns | 3ms, 459µs, 215ns | 4ms, 17µs, 114ns |
| Php-di(Reflection, Singleton) | ^7.0 | 427µs, 722ns | 390µs, 52ns | 662µs, 88ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 287µs, 794ns | 1ms, 253µs, 128ns | 1ms, 420µs, 21ns |
| Quickly(Compiled, Singleton) | dev-master | 878µs, 477ns | 864µs, 28ns | 901µs, 937ns |
| Quickly(Configured, Singleton) | dev-master | 937µs, 724ns | 897µs, 169ns | 967µs, 25ns |
| Quickly(Reflection, Singleton) | dev-master | 692µs, 605ns | 665µs, 903ns | 822µs, 67ns |
| Symfony(Compiled, Singleton) | ^7.0 | 615µs, 549ns | 591µs, 993ns | 655µs, 174ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 449µs, 466ns | 411µs, 987ns | 676µs, 870ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 460µs, 743ns | 407µs, 934ns | 802µs, 40ns |
| Zen(Compiled, Singleton) | ^3.1 | 861µs, 525ns | 745µs, 58ns | 1ms, 582µs, 145ns |

</details>

### 🚀 z26 startup

Large dependency graph including a total of 26 classes. Skipped for all but the fastest DI-Containers for runtime reasons. (includes container startup time)

![🚀 z26 startup](images/speed_comparison_with_startup_z26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Laminas-servicemanager(Reflection, Singleton) | ^4.4 | 1ms, 91µs, 909ns | 953µs, 912ns | 2ms, 168µs, 893ns |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 761µs, 553ns | 3ms, 532µs, 886ns | 5ms, 19µs, 903ns |
| Php-di(Reflection, Singleton) | ^7.0 | 749µs, 993ns | 571µs, 12ns | 2ms, 140µs, 998ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 332µs, 688ns | 1ms, 284µs, 837ns | 1ms, 561µs, 164ns |
| Quickly(Compiled, Singleton) | dev-master | 824µs, 975ns | 775µs, 98ns | 846µs, 147ns |
| Quickly(Configured, Singleton) | dev-master | 2ms, 204µs, 608ns | 2ms, 97µs, 845ns | 2ms, 979µs, 40ns |
| Quickly(Reflection, Singleton) | dev-master | 1ms, 291µs, 847ns | 1ms, 200µs, 199ns | 1ms, 948µs, 118ns |
| Symfony(Compiled, Singleton) | ^7.0 | 845µs, 766ns | 822µs, 67ns | 879µs, 49ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 1ms, 254µs, 320ns | 804µs, 901ns | 3ms, 201µs, 961ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 1ms, 182µs, 579ns | 970µs, 840ns | 2ms, 983µs, 93ns |
| Zen(Compiled, Singleton) | ^3.1 | 1ms, 193µs, 594ns | 931µs, 978ns | 3ms, 52µs, 949ns |

</details>

### 📊 zin26

Large interface-based dependency graph including a total of 26 interfaces. Skipped for all but the fastest DI-Containers for runtime reasons. (excluding container startup time)

![📊 zin26](images/speed_comparison_interfaces_without_startup26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 982µs, 400ns | 3ms, 865µs, 957ns | 4ms, 395µs, 961ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 319µs, 885ns | 1ms, 294µs, 136ns | 1ms, 353µs, 979ns |
| Quickly(Compiled, Singleton) | dev-master | 593µs, 256ns | 578µs, 880ns | 622µs, 34ns |
| Quickly(Configured, Singleton) | dev-master | 3ms, 771µs, 615ns | 3ms, 678µs, 83ns | 3ms, 990µs, 888ns |
| Symfony(Compiled, Singleton) | ^7.0 | 558µs, 137ns | 541µs, 925ns | 581µs, 26ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 532µs, 698ns | 483µs, 989ns | 803µs, 947ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 466µs, 561ns | 400µs, 66ns | 890µs, 16ns |
| Zen(Compiled, Singleton) | ^3.1 | 882µs, 792ns | 788µs, 927ns | 1ms, 604µs, 80ns |

</details>

### 🚀 zin26 startup

Large interface-based dependency graph including a total of 26 interfaces. Skipped for all but the fastest DI-Containers for runtime reasons. (includes container startup time)

![🚀 zin26 startup](images/speed_comparison_interfaces_with_startup26.jpg)

<details>
<summary>View results</summary>

| Container | Version | Average | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Nette-di(Compiled, Singleton) | ^3.2 | 3ms, 988µs, 3ns | 3ms, 913µs, 164ns | 4ms, 455µs, 89ns |
| Pimple(Configured, Singleton) | ^3.5 | 1ms, 419µs, 734ns | 1ms, 366µs, 138ns | 1ms, 590µs, 13ns |
| Quickly(Compiled, Singleton) | dev-master | 663µs, 89ns | 639µs, 915ns | 712µs, 871ns |
| Quickly(Configured, Singleton) | dev-master | 5ms, 175µs, 137ns | 4ms, 501µs, 819ns | 10ms, 116µs, 100ns |
| Symfony(Compiled, Singleton) | ^7.0 | 793µs, 719ns | 738µs, 143ns | 952µs, 959ns |
| Yiisoft-di(Configured, Singleton) | ^1.4 | 1ms, 247µs, 906ns | 1ms, 13µs, 994ns | 3ms, 192µs, 901ns |
| Yiisoft-di(Reflection, Singleton) | ^1.4 | 627µs, 827ns | 488µs, 996ns | 1ms, 742µs, 124ns |
| Zen(Compiled, Singleton) | ^3.1 | 572µs, 943ns | 416µs, 40ns | 1ms, 680µs, 135ns |

</details>

Questions, issues, and new containers are welcome!
