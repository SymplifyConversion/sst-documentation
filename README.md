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
```
NodeJS:

```js
describe("Audience validation", () => {
    const response = fetch('https://raw.githubusercontent.com/SymplifyConversion/sst-sdk-nodejs/main/test/sdk_config.json');
    const testData = JSON.parse(response.json());

    for (const s of testData) {
        if (s.skip) {
            console.log(`skipping test '${s.suite_name}' (because ${s.skip})`);
            continue;
        }

        describe(s.suite_name, () => {
            for (const i in s.test_cases) {
                const t = s.test_cases[i];
                const desc = `expression JSON '${t.audience_string}' should error with '${t.expect_error}'`;
                test(desc, () => {
                    expect(() => new Audience(t.audience_string)).toThrow(t.expect_error);
                });
            }
        });
    }
});
```
## 