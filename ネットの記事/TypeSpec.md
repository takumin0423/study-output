## これはなに
- [TypeSpec](https://typespec.io/)のドキュメントを読んでいくので、まとめる
- 読んだら追記していく

## TypeSpecってなに
- Microsoftにより作られた、TypeScriptベースのOpenAPI定義書やスキーマ定義などに変換できる言語
- .yamlよりも簡潔な構文が用意されている
```yaml
openapi: 3.0.0
info:
  title: (title)
  version: 0.0.0
tags: []
paths:
  /stores:
    get:
      operationId: Stores_list
      parameters:
        - name: filter
          in: query
          required: true
          schema:
            type: string
      responses:
        '200':
          description: The request has succeeded.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Store'
  /stores/{id}:
    get:
      operationId: Stores_read
      parameters:
        - name: id
          in: path
          required: true
          schema:
            $ref: '#/components/schemas/Store'
      responses:
        '200':
          description: The request has succeeded.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Store'
components:
  schemas:
    Address:
      type: object
      required:
        - street
        - city
      properties:
        street:
          type: string
        city:
          type: string
    Store:
      type: object
      required:
        - name
        - address
      properties:
        name:
          type: string
        address:
          $ref: '#/components/schemas/Address'
```

```tsp
import "@typespec/json-schema";

using TypeSpec.JsonSchema;

@jsonSchema
namespace Schemas;

model Person {
  name: string;
  address: Address;
  @uniqueItems nickNames?: string[];
  cars?: Car[];
}

model Address {
  street: string;
  city: string;
  country: string;
}

model Car {
  kind: "ev" | "ice";
  brand: string;
  @minValue(1900) year: int32;
}
```
