# sst-documentation
Symplify Server-Side Documentation repository containing documentation and test data for server-side testing

Documentation in `docs` folder should be linked from each SST-SDK repo.

## Tests
The SST-SDKs uses the same data for testing, this ensures that all languages have the same result.

When building a test, fetch the provider data from each file in raw format.

For example in PHP:
```php 
public function audienceTestAttributesProvider(): array {
$json      = file_get_contents('https://raw.githubusercontent.com/SymplifyConversion/sst-sdk-nodejs/main/test/sdk_config.json');
$attributesData = json_decode($json, true);

        $attributes = array();

        foreach ($attributesData as $attributeData) {
            $attributes[$attributeData['suite_name']] = [
                $attributeData['audience_json'] ?: null,
                $attributeData['test_cases'] ?? array(),
            ];
        }

        return $attributes;
    }
     
/**
 * @dataProvider audienceTestAttributesProvider
 * @param array<mixed> $audience_json
 * @param array<mixed> $test_cases
 */
 // Insert Test here using the above params. These are created with the provider above
}

```
NodeJS:

```js
describe("Audience validation", () => {
    const response = fetch('https://raw.githubusercontent.com/SymplifyConversion/sst-sdk-nodejs/main/test/sdk_config.json');
    const testData = JSON.parse(response.json());

    // Insert Test code here
});
```
## 