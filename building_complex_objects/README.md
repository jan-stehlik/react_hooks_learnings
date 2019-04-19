### Don't use useMemo to build complex object that depend on asynchrnous operation

```
const [myData, setMyData] = useState(null)
const complexObject = useMemo(() => buildComplexObject(myData), [myData])

useEffect(() => {
	const fetchData = async () => {
		const response = await fetch(url)

		setMyData(response.data)
	}

	fetchData()
}, [url])
```

`buildComplexObject` in this example will be run at least twice - once when `myData` is `null` and once when `myData` equals `response.data`. Given react [doesn't guarantee](https://reactjs.org/docs/hooks-faq.html#how-to-memoize-calculations) that computation inside `useMemo` will run only once this approach is not good.

### Use useState instead

```
const [myData, setMyData] = useState(null)
const [complexObject, setComplexObject] = useState(null)

useEffect(() => {
	const fetchData = async () => {
		const response = await fetch(url)

		setMyData(response.data)
		setComplexObject(buildComplexObject(response.data))
	}

	fetchData()
}, [url])
```

In this example `buildComplexObject` is run only once, or whenever `url` changes.
