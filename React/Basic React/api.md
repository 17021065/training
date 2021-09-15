# API
Có rất nhiều cách để truy vấn dữ liệu từ một API trong React, phổ biến nhất trong số đó là: **Axios**, **jQuery AJAX**, và **window.fetch**.

## XHR
```
class Searcher extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  const url = `${API_ENDPOINT}${this.props.searchTerm}`;

  componentDidMount() {
    var xhr = new XMLHttpRequest();
    var json_obj, status = false;
    xhr.open("GET", url, true);
    xhr.onload = function (e) {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
          var json_obj = JSON.parse(xhr.responseText);
          status = true;
          this.setState({ json_obj });
        } else {
          console.error(xhr.statusText);
        }
      }
    }.bind(this);
    xhr.onerror = function (e) {
      console.error(xhr.statusText);
    };
    xhr.send(null);
  }

  render() {
    return null;
  }
}
```

## Fetch
```
class Searcher extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  const url = `${API_ENDPOINT}${this.props.searchTerm}`;

  componentDidMount() {
    fetch(url)
      .then(res => res.json())
      .then(
        (result) => this.setState({result.items});
        ,
        (error) => console.log(error);
      )
  }

  render() {
    return null;
  }
}
```

## Axios
Axios là một thư viện chuyên dùng để truy vấn API.
```
class Searcher extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  const url = `${API_ENDPOINT}${this.props.searchTerm}`;

  componentDidMount() {
    axios.get(url)
      .then(res => res.json())
      .then(result => this.setState({result.items}))
      .catch(error => console.log(error))
  }

  render() {
    return null;
  }
}
```