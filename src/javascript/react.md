- 使用 normalize 转换默认 value
```js
const { Checkbox, Form } = antd;
const CheckboxGroup = Checkbox.Group;
const FormItem = Form.Item;

const options = [
  { label: 'All', value: 'All' },
  { label: 'Apple', value: 'Apple' },
  { label: 'Pear', value: 'Pear' },
  { label: 'Orange', value: 'Orange' },
];

class Demo extends React.Component {
  normalizeAll = (value, prevValue = []) => {
    if (value.indexOf('All') >= 0 && prevValue.indexOf('All') < 0) {
    	return ['All', 'Apple', 'Pear', 'Orange'];
    }
    if (value.indexOf('All') < 0 && prevValue.indexOf('All') >= 0) {
    	return [];
    }
    return value;
  };
  render() {
    return (
      <Form>
        <FormItem>
          {this.props.form.getFieldDecorator('fruits', {
            normalize: this.normalizeAll,
          })(<CheckboxGroup options={options} />)}
        </FormItem>
      </Form>
    );
  }
}

const WrappedDemo = Form.create()(Demo);

ReactDOM.render(<WrappedDemo />, mountNode);

```

- 使用 hooks 请求数据

```js
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";
import axios from 'axios';

function SearchResults() {
  const [data, setData] = useState({ hits: [] });
  const [query, setQuery] = useState('react');

  useEffect(() => {
    let ignore = false;

    async function fetchData() {
      const result = await axios('https://hn.algolia.com/api/v1/search?query=' + query);
      if (!ignore) setData(result.data);
    }

    fetchData();
    return () => { ignore = true; }
  }, [query]);

  return (
    <div>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <ul>
        {data.hits.map(item => (
          <li key={item.objectID}>
            <a href={item.url}>{item.title}</a>
          </li>
        ))}
      </ul>
    <div/>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<SearchResults />, rootElement);

```