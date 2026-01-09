<!DOCTYPE html>
<html lang="en">
<head>
    <title>Photo Gallery Project</title>
    
    
    <script src="https://cdn.tailwindcss.com"></script>
    
   
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    
    <!-- Babel for JSX -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
        
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1a1a1a;
        }
        ::-webkit-scrollbar-thumb {
            background: #333;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
      
        const { useState, useEffect } = React;

       
        const useFetch = (url) => {
            const [data, setData] = useState(null);
            const [loading, setLoading] = useState(true);
            const [error, setError] = useState(null);

            useEffect(() => {
              
                const fetchData = async () => {
                    setLoading(true);
                    setError(null);
                    try {
                        const response = await fetch(url);
                        if (!response.ok) {
                            throw new Error('Network response was not ok');
                        }
                        const result = await response.json();
                        setData(result);
                        setLoading(false);
                    } catch (err) {
               
                        setError('Failed to fetch');
                        setLoading(false);
                    }
                };

               
                fetchData();

               
                const handleOffline = () => {
               
                    setLoading(true);
                    setData(null); 
                    
                    
                    setTimeout(() => {
                        setLoading(false);
                        setError('Failed to fetch');
                    }, 1000); 
                };

                
                const handleOnline = () => {
                   
                    setLoading(true);
                    setError(null);
                    
                    fetchData();
                };

                
                window.addEventListener('online', handleOnline);
                window.addEventListener('offline', handleOffline);

               
                return () => {
                    window.removeEventListener('online', handleOnline);
                    window.removeEventListener('offline', handleOffline);
                };
            }, [url]);

            return { data, loading, error };
        };

       
        const App = () => {
            
			
            const { data: photos, loading, error } = useFetch('https://jsonplaceholder.typicode.com/photos?_limit=100');

            return (
                <div className="min-h-screen bg-[#1a1a1a] text-white font-sans selection:bg-pink-500 selection:text-white">
                    {/* Header */}
                    <header className="p-6 border-b border-gray-800 bg-[#1a1a1a] sticky top-0 z-10 shadow-md">
                        <h1 className="text-3xl font-bold text-center">Photos</h1>
                    </header>

                    
                    <main className="p-4 flex justify-center items-center min-h-[80vh]">
                        
                        
                        {loading && (
                            <div className="flex flex-col items-center justify-center space-y-4">
                          
                       
                                <h2 className="text-2xl font-semibold tracking-wider">Loading...</h2>
                            </div>
                        )}

                       
                        {!loading && error && (
                            <div className="flex flex-col items-center justify-center space-y-2 text-center">
                                <h2 className="text-2xl font-bold tracking-wide">Error : {error}</h2>
                                
                            </div>
                        )}

                      
                        {!loading && !error && photos && (
                            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 max-w-7xl w-full">
                                {photos.map((photo) => (
                                    <div 
                                        key={photo.id} 
                                        className="bg-black border border-gray-700 hover:border-pink-500 transition-colors duration-300 flex flex-col items-center p-4 shadow-lg group cursor-pointer"
                                    >
                                       
                                        <div className="relative w-full aspect-square overflow-hidden bg-gray-800 mb-4">
                                            <img 
                                                src={"https://th.bing.com/th/id/OIP.U1MdjaXPL00AT-yoS2wuhAHaEo?w=215&h=180&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3"} 
                                                alt={photo.title}
                                                className="w-full h-full object-cover transform group-hover:scale-105 transition-transform duration-500"
                                            />
                                        </div>

                                       
                                        <p className="text-center text-sm text-gray-300 line-clamp-2 px-2">
                                            {photo.title}
                                        </p>
                                    </div>
                                ))}
                            </div>
                        )}
                    </main>
                </div>
            );
        };

        // Application Render logic
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
