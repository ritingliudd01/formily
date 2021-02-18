---
order: 2
---

# ObjectField

## 描述

作为@formily/core 的 [createObjectField](https://core.formilyjs.org/api/models/form#createobjectfield) React 实现，它是专门用于将 ViewModel 与输入控件做绑定的桥接组件，ArrayField 组件属性参考[IFieldFactoryProps](https://core.formilyjs.org/api/models/form#ifieldfactoryprops)

<Alert>
我们在使用 ObjectField 组件的时候，一定要记得传name属性。同时要使用render props形式来组织子组件
</Alert>

## 签名

```ts
type ObjectField = React.FC<IFieldFactoryProps>
```

## 用例

```tsx
import React from 'react'
import { createForm } from '@formily/core'
import { FormProvider, Field, ObjectField } from '@formily/react'
import { Input, Button, Space } from 'antd'
import 'antd/lib/input/style'
import 'antd/lib/button/style'
import 'antd/lib/space/style'

const form = createForm()

export default () => (
  <FormProvider form={form}>
    <ObjectField name="object">
      {(field) => {
        return (
          <>
            <div>
              {Object.keys(field.value || {}).map((key) => (
                <div
                  key={key}
                  style={{ display: 'flex-block', marginBottom: 10 }}
                >
                  <Space>
                    <Field
                      name={key}
                      component={[Input, { placeholder: key }]}
                    />
                    <Button
                      onClick={() => {
                        field.removeProperty(key)
                      }}
                    >
                      Remove
                    </Button>
                  </Space>
                </div>
              ))}
            </div>
            <Space>
              <Field
                name="propertyName"
                basePath={''}
                required
                component={[Input, { placeholder: 'Property Name' }]}
              />
              <Button
                onClick={() => {
                  const name = form.values.propertyName
                  if (name && !form.existValuesIn(`object.${name}`)) {
                    field.addProperty(name, '')
                    form.deleteValuesIn('propertyName')
                  }
                }}
              >
                Add
              </Button>
            </Space>
          </>
        )
      }}
    </ObjectField>
  </FormProvider>
)
```