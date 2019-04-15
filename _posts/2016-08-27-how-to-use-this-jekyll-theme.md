    //Singleton
    #include <memory>
    #inlcude <mutex>
    
    ////////////////////////////////////////////////////////////////
    //懒汉（） 线程不安全 
    class MySingleton{
    
    private:
    	MySingleton(){}
    	static MySingleton *m_msOnlyOnce;
    
    public:
    	static MySingleton *getInstance(){
    		if ( m_msOnlyOnce == NULL )
    		{
    			m_msOnlyOnce = new MySingleton();
    		}
    		return m_msOnlyOnce
    	}
    
    	void otherMethod();
    };
    
    ///////////////////////////////////////////////////////////////
    //饿汉模式
    class MySingleton_ehan{
    
    private:
    	MySingleton_ehan(){}
    	static MySingleton_ehan *m_msOnlyOnce;
    
    public:
    	static MySingleton_ehan *getInstance(){
    		return m_msOnlyOnce ;
    	}
    
    	void otherMethod();
    };
    
    static MySingleton_ehan *m_msOnlyOnce = new MySingleton_ehan();
    
    /////////////////////////////////////////////////////////////////
    
    //smart pointer 
    //借鉴别人的代码 
    class CSingleton{
    
    public:
    	virtual ~CSingleton();
    	static CSingleton &GetInstance();
    
    private:
    	static std::unique_ptr<CSingleton> m_instance;
    	static std::onec_flag m_onceFlag;
    	CSingleton(void);
    	CSingleton(const CSingleton &src);
    	CSingleton& operator=(const CSingleton& rhs);
    };
    
    
    std::unique_ptr<CSingleton> CSingleton::m_instance;
    std::onec_flag CSingleton::m_onceFlag;
    
    CSingleton& CSingleton::GetInstance(){
    	std::call_once(m_onceFlag,
    		[] {
    			m_instance.reset(new CSingleton);
    		});
    	return *m_instance.get();
    }
    ///////////////////////////////////////////////////////////////////
