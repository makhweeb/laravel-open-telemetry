---
title: Installation & setup
weight: 4
---

You can install the package via composer:

```bash
composer require spatie/laravel-open-telemetry
```

## Run the installer command

Next, you should publish the configuration using this command

```bash
php artisan open-telemetry:install
```

This command will create a config file in `config/open-telemetry.php` looks like:

```php
return [
    /*
     * This value will be sent along with your trace.
     *
     * When set to `null`, the app name will be used
     */
    'default_trace_name' => null,

    /*
     * A driver is responsible for transmitting any measurements.
     */
    'drivers' => [
        Spatie\OpenTelemetry\Drivers\HttpDriver::class => [
            'url' => 'http://localhost:9412/api/v2/spans',
        ],
    ],

    /*
     * This class determines if your measurements should actually be sent
     * to the reporting drivers.
     */
    'sampler' => Spatie\OpenTelemetry\Support\Samplers\AlwaysSampler::class,

    /*
     * Tags can be added to any measurement. These classes will determine the
     * values of the tags when a new trace starts.
     */
    'trace_tag_providers' => [
        \Spatie\OpenTelemetry\Support\TagProviders\DefaultTagsProvider::class,
    ],

    /*
     * Tags can be added to any measurement. These classes will determine the
     * values of the tags when a new span starts.
     */
    'span_tag_providers' => [

    ],

    'queue' => [
        /*
         * When enabled, any measurements (spans) you make in a queued job that implements
         * `TraceAware` will automatically belong to the same trace that was
         * started in the process that dispatched the job.
         */
        'make_queue_trace_aware' => true,

        /*
         * When this is set to `false`, only jobs the implement
         * `TraceAware` will be trace aware.
         */
        'all_jobs_are_trace_aware_by_default' => true,

        /*
         *  When set to `true` all jobs will
         *  automatically start a span.
         */
        'all_jobs_auto_start_a_span' => true,

        /*
         * These jobs will be trace aware even if they don't
         * implement the `TraceAware` interface.
         */
        'trace_aware_jobs' => [

        ],

        /*
         * These jobs will never trace aware, regardless of `all_jobs_are_trace_aware_by_default`.
         */
        'not_trace_aware_jobs' => [

        ],
    ],

    /*
     * These actions can be overridden to have fine-grained control over how
     * the package performs certain tasks.
     *
     * In most cases, you should use the default values.
     */
    'actions' => [
        'make_queue_trace_aware' => Spatie\OpenTelemetry\Actions\MakeQueueTraceAwareAction::class,
    ],

    /*
     * This class determines how the package measures time.
     */
    'stopwatch' => Spatie\OpenTelemetry\Support\Stopwatch::class,

    /*
     * This class generates IDs for traces and spans.
     */
    'id_generator' => Spatie\OpenTelemetry\Support\IdGenerator::class,
];
```

It will also copy a service provider to `app/Providers/OpenTelemetryServiceProvider`. This provider contains code to measure requests.

## Setting up Jaeger via Docker locally

This package transmits results to an OpenTelemetry reporting tool like [Jaeger](https://www.jaegertracing.io).

The easiest way to set up Jaeger locally is by using the Docker composer file found in [this project on GitHub](https://github.com/prondubuisi/otel-php-laravel-basic-example).
