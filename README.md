# Fake-Car
Faker provider for fake car data

[![Build Status](https://travis-ci.org/pelmered/fake-car.svg?branch=master)](https://travis-ci.org/pelmered/fake-car)
[![Monthly Downloads](https://poser.pugx.org/pelmered/fake-car/d/monthly)](//packagist.org/packages/pelmered/fake-car)
[![Latest Stable Version](https://poser.pugx.org/pelmered/fake-car/v/stable)](https://packagist.org/packages/pelmered/fake-car)
[![Latest Unstable Version](https://poser.pugx.org/pelmered/fake-car/v/unstable)](//packagist.org/packages/pelmered/fake-car)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/pelmered/fake-car/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/pelmered/fake-car/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/pelmered/fake-car/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/pelmered/fake-car/?branch=master)
[![License](https://poser.pugx.org/pelmered/fake-car/license)](https://packagist.org/packages/pelmered/fake-car)


## Installation

To install as a dev dependency run:
```sh
composer require pelmered/fake-car --dev
```
Remove the `--dev` flag if you need it in production.

## Basic Usage
```php
$faker = (new \Faker\Factory())::create();
$faker->addProvider(new \Faker\Provider\Fakecar($faker));


// generate matching automobile brand and model of car as a string
echo $faker->vehicle; //Volvo 740

// generate matching automobile brand and model of car as an array
echo $faker->vehicleArray; //[ 'brand' => 'Hummer', 'model' => 'H1' ]

// generate only automobile brand
echo $faker->vehicleBrand; //Ford

// generate automobile manufacturer and model of car
echo $faker->vehicleModel; //488 Spider

// generate Vehicle Identification Number(VIN) - https://en.wikipedia.org/wiki/Vehicle_identification_number
echo $faker->vin; //d0vcddxpXAcz1utgz

// generate automobile registration number
echo $faker->vehicleRegistration; //ABC-123

// generate automobile registration number with custom format
echo $faker->vehicleRegistration('[A-Z]{2}-[0-9]{5}'); //AB-12345

// generate automobile model type
echo $faker->vehicleType; //hatchback

// generate automobile fuel type
echo $faker->vehicleFuelType; //diesel

// generate automobile door count
echo $faker->vehicleDoorCount; //4

// generate automobile seat count
echo $faker->vehicleSeatCount; //5

// generate automobile properties
echo $faker->vehicleProperties; //['Towbar','Aircondition','GPS', 'Leather seats']

// generate automobile gear type (manual or automatic)
echo $faker->vehicleGearBoxType; //manual
```

### Laravel factory example

```php
<?php
namespace Database\Factories;

use App\Models\Vehicle;
use Faker\Provider\Fakecar;
use Illuminate\Database\Eloquent\Factories\Factory;

class VehicleFactory extends Factory
{
    /**
     * The name of the factory's corresponding model.
     *
     * @var string
     */
    protected $model = Vehicle::class;

    /**
     * Define the model's default state.
     *
     * @return array
     */
    public function definition()
    {
        $this->faker->addProvider(new Fakecar($this->faker));
        $vehicle = $this->faker->vehicleArray();

        return [
            'vehicle_type'    => 'car',
            'vin'             => $this->faker->vin,
            'registration_no' => $this->faker->vehicleRegistration,
            'chassis_type'    => str_replace(' ', '_', $this->faker->vehicleType),
            'fuel'            => $this->faker->vehicleFuelType,
            'brand'           => $vehicle['brand'],
            'model'           => $vehicle['model'],
            'year'            => $this->faker->biasedNumberBetween(1990, date('Y'), 'sqrt'),
        ];
    }
}
```
