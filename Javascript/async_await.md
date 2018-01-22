## async, await

만약 어떤 함수에서 promise가 리턴되고, 그 promise 이후에 어떤 일을 하고 싶다면 기존에는 .then을 해서 처리했다.

그러나 ES6부터는 await을 통해 그 promise를 기다릴 수 있고, 반환 내용을 받아서 처리할 수 있다.

그리고 await는 async 함수 내부에서만 사용 가능하다.

example

	async function registerInitialExchangeRate() {
	    // 초기 세팅에 필요한 가상화폐별 데이터를 가져온다.
	    const tickers = await poloniex.getTickers();
	    
	    const keys = Object.keys(tickers);
	    
	    const promises = keys.map( key => {
	        const ticker = tickers[key];
	        const data = Object.assign({name: key}, ticker);
	        const exchangeRate = new ExchangeRate(data);
	        return exchangeRate.save();
	    });
	
	    try {
	        Promise.all(promises);
	    }catch (e) {
	        console.error(e);
	    }
	}