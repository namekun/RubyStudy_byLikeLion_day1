챗봇으로 루비를 시작해보자!

안녕

   	puts '안녕하세요!'

점심메뉴

    menu = ["20층", "순남시래기", "양자강", "한우","버거킹"]
    lunch = menu.sample #랜덤으로 하나뽑기
    puts lunch

점심 저녁 메뉴선택

    # 점심에는 ? 을 먹고 저녁에는 ? 를 드세요
    # 조건 : .sample 함수는 1번만 사용가능
    
    menu = ["20층", "순남시래기", "양자강", "시골집", "버거킹", "편의점도시락"]
    eat = menu.sample(2) #랜덤으로 하나뽑기
    
    print "점심에는 #{eat[0] }를(을) 먹고 저녁에는 #{eat[1] }를(을) 드세요.."

로또 번호 추출

    number = *(1..45)
    lotto = number.sample(6).sort
    puts "이번주 추천 로또 번호는" + lotto.to_s + "입니다."

미세먼지경보

    require 'httparty'
    url = URI.encode("http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getMsrstnAcctoRltmMesureDnsty?stationName=강남구&dataTerm=MONTH&numOfRows=100&ServiceKey=")+key
    response = HTTParty.get(url)
    dust = response['response']['body']['items']['item'][0]['pm10Value'].to_i
    
    print "현재 미세먼지 농도는 #{dust}이며, 미세먼지 상태는 "		
    case dust
    	when 0..30 # 0-30까지
    	  print "좋음입니다."	
    	when 0..80
    		print "보통입니다."
    	when 0..150
    		print "나쁨입니다."
    	else 		
            print "매우나쁨입니다."
    end

Kospi Point

    require 'httparty'
    require 'nokogiri'
    
    response = HTTParty.get("http://finance.daum.net/quote/kospi.daum?nil_stock=refresh")
    kospi = Nokogiri::HTML(response)
    
    # f12 누르고 코스피부분놀러서 받아오는것
    result = kospi.css("#hyenCost > b")
    print "오늘의 코스피지수는 #{result.text}입니다."

exchange Rate

    require 'httparty'
    require 'nokogiri'
    
    responseUS = HTTParty.get("http://finance.daum.net/exchange/exchangeDetail.daum?code=USD")
    responseJP = HTTParty.get("http://finance.daum.net/exchange/exchangeDetail.daum?code=JPY")
    responseCN = HTTParty.get("http://finance.daum.net/exchange/exchangeDetail.daum?code=CNY")
    exchangeUS = Nokogiri::HTML(responseUS)
    exchangeJP = Nokogiri::HTML(responseJP)
    exchangeCN = Nokogiri::HTML(responseCN)
    
    
    # f12 누르고 환율부분놀러서 받아오는것
    resultUSD = exchangeUS.css("#hyenCost > b")
    resultJPY = exchangeJP.css("#hyenCost > b")
    resultCNY = exchangeCN.css("#hyenCost > b")
    
    puts "금일 미국의 환율은 #{resultUSD.text}원 입니다"
    puts "금일 일본의 환율은 #{resultJPY.text}원 입니다"
    print "금일 중국의 환율은 #{resultCNY.text}원 입니다"

weather

    require 'httparty'
    require 'nokogiri'
    
    responseNaverMo =HTTParty.get("https://weather.naver.com/rgn/cityWetrCity.nhn?cityRgnCd=CT001013")
    responseNaverAf =HTTParty.get("https://weather.naver.com/rgn/cityWetrCity.nhn?cityRgnCd=CT001013")
    responseNaverWthinfo  =HTTParty.get("https://weather.naver.com/rgn/cityWetrCity.nhn?cityRgnCd=CT001013")
    
    moweatherNV = Nokogiri::HTML(responseNaverMo)
    afweatherNV = Nokogiri::HTML(responseNaverAf)
    weatherinfo = Nokogiri::HTML(responseNaverWthinfo)
    
    resultwthmo = moweatherNV.css("#content > table.tbl_weather.tbl_today3 > tbody > tr > td:nth-child(1) > div:nth-child(1) > ul > li.nm > span")
    resultwthaf = afweatherNV.css("#content > table.tbl_weather.tbl_today3 > tbody > tr > td:nth-child(1) > div:nth-child(3) > ul > li.nm > span")
    resultfeel = weatherinfo.css("#content > div.w_now2 > ul > li:nth-child(1) > div > em > strong")
    
    print "금일 강남구 역삼동의 오전은 #{resultwthmo.text}℃이고, 오후는 #{resultwthaf.text}℃ 이며, 오늘은 #{resultfeel.text}입니다."

Rain Rate

    require 'httparty'
    require 'nokogiri'
    
    responseRainMo =HTTParty.get("https://weather.naver.com/rgn/cityWetrCity.nhn?cityRgnCd=CT001013")
    responseRainAf =HTTParty.get("https://weather.naver.com/rgn/cityWetrCity.nhn?cityRgnCd=CT001013")
    
    
    rainmo = Nokogiri::HTML(responseRainMo)
    rainaf = Nokogiri::HTML(responseRainAf)
    
    resultRainMo = rainmo.css("#content > table.tbl_weather.tbl_today3 > tbody > tr > td:nth-child(1) > div:nth-child(1) > ul > li.info > span > strong")
    resultRainAf = rainaf.css("#content > table.tbl_weather.tbl_today3 > tbody > tr > td:nth-child(1) > div:nth-child(3) > ul > li.info > span > strong")
    
    print "금일 오전 강수확률은 #{resultRainMo.text}이고, 오후는 #{resultRainAf.text} 입니다."


